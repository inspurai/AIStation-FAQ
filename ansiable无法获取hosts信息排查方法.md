
排查发现网关信息配置错误，导致ansible无法获取hosts信息，更新/etc/hosts文件失败，具体测试处理过程如下：

#第一步：检查是否能够获取网关信息

    mkdir -p /etc/ansible
    cp inventory.ini  /etc/ansible/hosts
    ansible all -m setup
    ansible host_name -m setup | grep gateway
    
#第二步：在kubespray目录下检查否能够获取网关信息

    cd ./deploy-script/kubespray/
    ansible -i ./iventory/sample/inventory.ini all -m setup 
    ansible -i ./iventory/sample/inventory.ini host_name -m setup | grep gateway
    
#第三步：再次执行安装第7步
