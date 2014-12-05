Galera-mysql
============

# I. Giới Thiệu 
Galera được sử dụng để đồng bộ cơ sở dữ liệu Mysql.

# II. Tiến Hành 

### 2.1 Mô tả bài lab

Có 1 server (Server1) cài Ubuntu 14.04 cài đặt mysql-server phiên bản 5.5, trong mysql 5.5 đã có các database có sẵn. 

Mục tiêu: Xây dựng ra thêm server 2 sẽ đồng bộ toàn bộ dữ liệu trên Server1 sang Server2.

### 2.2 Các bước chuẩn bị

Server2: 

  +/ OS: Ubuntu 14.04

  +/ IP: 10.145.37.51
  
Server1: 

  +/ OS: Ubuntu 14.04

  +/ IP: 10.145.37.50
  
  +/ Đã cài đặt sẵn mysql server 5.5 có sẵn cơ sở dữ liệu.

### 2.3 Các bước thực hiện 

#### 2.3.1 Tiến hành cài đặt trên Server1 

B1: Cài đặt mysql-server. mysql-client  và mysql-client-core phiên bản 5.6 

    apt-get install mysql-server-5.6 mysql-client-5.6 mysql-client-core-5.6

B2: Tải galera và mysql-server-wsrep 5.6 từ website Coderships

    wget https://launchpad.net/galera/3.x/25.3.5/+download/galera-25.3.5-amd64.deb
    wget https://launchpad.net/codership-mysql/5.6/5.6.16-25.5/+download/mysql-server-wsrep-5.6.16-25.5-amd64.deb

B3: Tiếp tục cài gói sau: 

    apt-get install libssl0.9.8  

B4: Giải nén và cài đặt các gói vừa tải về 

    dpkg -i galera-25.3.5-amd64.deb
    dpkg --force-all -i mysql-server-wsrep-5.6.16-25.5-amd64.deb

B5: Chỉnh sửa lại file cấu hình: vi /etc/mysql/conf.d/wsrep.cnf
  
    wsrep_provider=/usr/lib/galera/libgalera_smm.so
    bind-address=0.0.0.0
    wsrep_node_incoming_address=10.145.37.50
    wsrep_cluster_address="gcomm://"

B6: Restart lại dịch vụ mysql

    # service mysql restart

#### 2.3.2 Tiến hành cài đặt trên Server2 

B1: Cài đặt mysql-server 

    apt-get install mysql-server
    apt-get install mysql-server-5.6 mysql-client-5.6 mysql-client-core-5.6

B2: Tiến hành cài đặt giống Server1 từ bước 2 đến bước 4

B3: Chỉnh sửa lại file cấu hình: vi /etc/mysql/conf.d/wsrep.cnf

    wsrep_provider=/usr/lib/galera/libgalera_smm.so
    bind-address=0.0.0.0
    wsrep_node_incoming_address=10.145.37.51
    wsrep_cluster_address="gcomm://10.145.37.50"

B4: Restart lại dịch vụ mysql

    # service mysql restart

#### 2.3.3 Quay lại Server1 

B1 Chỉnh sửa file sau: /etc/mysql/conf.d/wsrep.cnf. Tìm đến dòng sau dưới đây và đặt IP Server2 

    wsrep_cluster_address="gcomm://10.145.37.51"

B2: Restart lại mysql

   # service mysql restart

Sau khi Restart lại mysql trên server 1, quay lại server 2 restart lại mysql lần nữa và kiểm tra xem database đã được đồng bộ chưa. 

Bài lab đã kết thúc!

# Tài Liệu tham khảo 

http://www.fromdual.com/ugly-way-to-install-mysql-galera-cluster-5.6-on-ubuntu-14.04

http://behindtheracks.com/2014/04/mysql-high-availability-with-galera-and-haproxy/
