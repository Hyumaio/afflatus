# docker


## docker run ...
* -d: detached mode，后台运行模式。
* -p: port remapping，例如 4000:80 代表将 container 的 80 端口重映射到外部 4000 端口。
* docker 在 build 时就使用了 Dockerfile（例如其中的环境变量），后续对 Dockerfile 进行修改并不会影响已经 build 完毕的 image。