# docker


## docker run ...
+ `-d`: detached mode，后台运行模式。
+ `-p`: port remapping，例如 4000:80 代表将 container 的 80 端口重映射到外部 4000 端口。
+ docker 在 build 时就使用了 Dockerfile（例如其中的环境变量），后续对 Dockerfile 进行修改并不会影响已经 build 完毕的 image。
+ `-it <image> bash`，进入容器。

## docker-compose up ...

+ Volumes 选项默认是从容器外部挂载到容器内部，所以如果有配置文件需要从容器内部挂载出来时，要保证容器外部挂载目录下先有一份一模一样的文件，否则 compose 启动时容器外部挂载目录为空，就会把空挂载到容器内部，导致容器内部对应的文件消失；data 和 logs 不需要这样处理的原因是，这两个目录第一次运行时本来就是要求置为空的，后续重新运行时，也会把容器外部已经有的 data 和 logs 挂载进去。还有一点是，data 和 logs 是不会手动去修改的，而挂载的配置文件是需要修改的，所以如果后续重新打镜像时容器内部的这些需要挂载的文件有更改，那么一定记住容器外部挂载目录下一定要先有一份一模一样的文件，否则容器外部的文件内容会挂载到容器内部，将其覆盖，导致修改失效。

## docker-compose expose/ports
+ expose 是把此容器的端口暴露给其他内部容器，事实上因为 --icc 参数默认为 True，所以即使没有显式设置此值，其他内部容器也能访问此端口，此时 expose 只是相当于一种标注提示，并没有实际效用。expose 的容器可以不设置 ports，即只暴露给内部容器使用。
+ ports 是把此容器的端口暴露给外界，并且 ports 会隐式设置 expose。

## docker save/load
+ `docker save <image:tag> | gzip > <filename.tgz>` 可以制作压缩镜像，同样使用 `docker load <image:tag>` 加载此镜像

## 在服务器上修改挂载的配置文件时，发现代码中并未生效

+ 第一种可能性(google)：docker挂载文件基于inode。vim等编辑工具保存文件时，并非直接保存，而是将一份新的临时文件覆盖了旧文件。对于inode而言，原文件并未被修改。解决办法:
	+ 换用nano等直接更新文件的编辑工具。  
	+ 改为挂载目录。   
	+ 修改vim配置，添加 `set backupcopy=yes`。
+ 第二种可能性：更改配置后，用 gunicorn 启动的 flask 程序需要重启，才能使代码生效。
+ 还未清楚是哪一种情况，使用重启容器的方法可以暂时解决这个问题。

## docker latest 标签的疑惑
+ `docker tag/build` 的时候如果未指定 tag，会自动使用 latest 作为 tag。
+ `docker pull` 的时候如果只指定了 registry，未指定 tag，会自动使用 latest tag。
+ latest 只是一个字面意思，并不代表它对应的 registry 是最新的版本，你可以将其他任意过时的版本 tag 设置为 latest。
+ latest tag 不会自动更新，如果你需要更新，请重新 docker pull。
