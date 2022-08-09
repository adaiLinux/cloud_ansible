## Install Telegraf/Categraf with Ansible

目前支持 Centos6/7

## Telegraf/Categraf ansible-playbook 使用说明

telegraf ansible 分为两类：

* 通用：`tags: ["common"]` ，基础监控，如 CPU、mem、disk、network 等；
* 拨测：`tags: ["op"]` ，定制化监控，如 zookeeper、elasticsearch、nginx 等；

## 使用方法（操作步骤）

1. 将目标主机加入 ansible inventory 文件（eg-hosts），如下：

   ```yaml
   [common]
   10.100.10.xxx     ansible_ssh_host=10.100.10.xxx
   
   [op]
   10.100.20.xxx      ansible_ssh_host=10.100.20.xxx		feature='broadcast'
   ```

2. 普通安装：

   ```bash
   $ ansible-playbook -i eg-hosts -l 10.100.10.xxx telegraf.yml --tags "common"
   ```

3. 定制化安装：

   ```bash
   $ ansible-playbook -i eg-hosts -l 10.100.20.xxx telegraf.yml --tags "op"
   ```

### 定制化安装说明

定制化安装主要是针对监控中间件的拨测服务器。

进行定制化部署之前，需要做如下操作：

1. 准备子配置文件，如 nginx.conf，nginx_vts.conf 等；

2. 修改 `roles/telegraf/tasks/update.yml` 中如下内容（with_items 部分）：

   ```yaml
   - name: update children telegraf conf
     template:
       src: "telegraf.d/{{ item }}.conf.j2"
       dest: "{{ install_dir }}/telegraf.d/{{ item }}.conf"
       mode: 0644
     with_items:
       - "nginx"
       - "nginx_vts"
     when: env == "op"
     register: telegraf_child_conf_stat
   ```

3. 部署子配置文件：

   ```bash
   $ ansible-playbook -i eg-hosts -l 10.100.20.xxx telegraf.yml -e action=update --tags "op"
   ```

## 补充说明

1. 关于 playbook 详情，请自行阅读 telegraf/categraf playbook；
2. 关于 ansible 使用，请阅读：[官方文档](https://docs.ansible.com/ansible/2.7/index.html) 。
3. 关于其他问题，请自行 Baidu/Google。


