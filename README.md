# doris
doris docker-compose部署
be目录  be程序存储数据及其配置文件，日志等
fe目录  fe程序存储数据及其配置文件，日志等
lib目录 doris be所需的二进制文件（官方镜像里BE二进制需cpu支持avx2指令集，若cpu不支持支持avx2指令集，官网下载no avx2版本二进制）
init_fe.sh  doris-fe启动脚本,mysql连接加上密码
entry_point.sh  doris-be容器内运行脚本，脚本里先跑了init_be.sh,然后一直检查be是否注册到了fe
init_be.sh  doris-be启动脚本,mysql连接加上密码
docker-compose.yaml   doris docker-compose
docker-compose_first_init.yaml 第一次初始化doris集群,无mysql密码,所有初始化脚本均为官方,连接mysql无密码
docker-compose-after-first.yaml 第一次初始化完成后，设置mysql root,admin密码,重启服务连接mysql需要密码,所有初始化脚本需做修改（mysql连接加上密码）映射到容器内

后续可以再做优化，将mysql密码写到配置文件，初始化脚本读取配置文件
be的两个脚本可以合一起，entry_point.sh与init_be.sh有很多重复地方
#初始化完成后，若设置mysql root密码
#entry_point.sh,init_be.sh,init_fe.sh三个脚本均需修改,加上mysql密码
......
......
docker_process_sql() {
  set +e
  mysql -uroot -P9030 -ppasswd -h${MASTER_FE_IP} --comments "$@" 2>/dev/null
}
......
......
