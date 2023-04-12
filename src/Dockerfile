FROM maven:3.8.6-openjdk-8
LABEL description="Docker image with Hadoop 3.3.2, OpneJDK8 and Maven 3.8.3"

# 设置 Java 环境变量
ENV JAVA_HOME="/usr/local/openjdk-8"
ENV PATH="${JAVA_HOME}/bin:$PATH"

# 更换镜像源为中科大源
RUN mv /etc/apt/sources.list /etc/apt/sources.list.backup && \
    echo "deb http://mirrors.ustc.edu.cn/debian stable main contrib non-free" >> /etc/apt/sources.list && \
    echo "# deb-src http://mirrors.ustc.edu.cn/debian stable main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.ustc.edu.cn/debian stable-updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "# deb-src http://mirrors.ustc.edu.cn/debian stable-updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "# deb http://mirrors.ustc.edu.cn/debian stable-proposed-updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "# deb-src http://mirrors.ustc.edu.cn/debian stable-proposed-updates main contrib non-free" >> /etc/apt/sources.list

# 安装 SSH 服务和 curl
RUN apt-get update && \ 
    apt-get install -y openssh-server curl vim

# 自启动 SSH 服务，并配置免密登陆
RUN echo "service ssh start" >> /root/.bashrc && \
    ssh-keygen -t rsa -f /root/.ssh/id_rsa -P '' && \
    cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys && \
    echo "Host *" >> /etc/ssh/ssh_config && \
    echo "    StrictHostKeyChecking no" >> /etc/ssh/ssh_config && \
    echo "    UserKnownHostsFile=/dev/null" >> /etc/ssh/ssh_config

# 下载安装 Hadoop
RUN curl -OL https://mirrors.ustc.edu.cn/apache/hadoop/common/hadoop-3.3.2/hadoop-3.3.2.tar.gz
RUN tar -zxvf hadoop-3.3.2.tar.gz && \
    mv hadoop-3.3.2 hadoop && \
    rm -rf hadoop-3.3.2.tar.gz

# 设置 Hadoop 环境变量
ENV HADOOP_HOME="/hadoop"
ENV HADOOP_CONF_DIR="/app/conf"
ENV PATH="${HADOOP_HOME}/sbin:${HADOOP_HOME}/bin:$PATH"

WORKDIR /app
