`ps -ef | grep xxx进程 | awk '{ print $2 }' | xargs kill -9`
