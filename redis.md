## 缓存穿透

一个比较简单的防止 db 穿透的方法是，当首次请求 cache 中不存在对应 result，访问 db 也没有取得对应 result 时，应往 cache 里添加一个标记例如 `NOT_EXISTS`，下一次访问 cache 的时候如果拿到这个标记，就不应该再去 db 访问；但是这个方法要求其他更新了这个请求对应的 result 的地方必须正确设置 cache，否则就永远获取不到 db 更新后的结果。

以 scheduler 为例子，对不存在的数据设置 `NOT_EXECUTED` cache，下一次请求过来时如果 cache 里取到这个值，则认为数据不存在直接返回，这就要求 executor 执行 job 后更新 cache，否则 cache 永远都是 `NOT_EXECUTED`，在理想情况下，多次请求后 cache 变成了 db 的一个 replica，executor 会不断往 cache 里写新数据，这样就再也不需要访问 db 了。

还有一种方法，类似于 scheduler 的方式，对不存在的数据设置一个值，但是有过期时间，这样不需要 db 更新时动态去更新 cache，相同的请求在过期时间内都无法穿透到 db，这个方法的缺点是无法保证数据的及时更新，当数据更新后，在过期时间里请求永远都得不到更新的数据，如果是冷数据，不需要很强的及时性的话，就可以使用这样的方式，如果及时性是刚需，那么就要保证更新 db 时及时更新 cache，这时候的过期时间可有可无，如果能保证 cache 更新的及时性，那么最好是不设置过期时间，否则设置过期时间会比较保险。

而 collector 防止穿透的方法不同，这边是设置一个 sync_key，当 sync_key 不存在时，全量同步所有 fixed_token，当 sync_key 存在时就不去访问 db，相当于 cache 是 db 的一个简单 replica，当 cache 不存在时就默认 db 也不存在了。这个过程比较重，必须保证 cache 的正确性，所以 fixed_token 在生成的时候设置 cache，在修改和删除的时候也应该更新对应 cache，保证 cache 的正确性，只有在需要的时候才去同步 cache。与 scheduler 的不同点在于，scheduler 有单独访问 db 的选项，因为不会全量更新，只会根据请求慢慢的把所有 db 的数据写入到 cache，executor 也会更新 cache；而 collector 没有单独访问 db 的选项，因为有全量更新，需要的时候全量更新一次，db 的数据即可全写入到 cache，fixed_token 生成、更新、删除时也会更新 cache。相同点在于，如果老数据全部写入到 cache 里了，就再也不用访问 db。

**PS: 这里讨论的都是数据 cache 没有过期时间的情况，如果 cache 有过期时间，那么要对空数据设置一个过期时间，因为 cache 无法保证一直存在，这就要求必须有单独访问 db 的选项。**