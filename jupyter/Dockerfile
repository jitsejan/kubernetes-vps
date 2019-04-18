FROM jupyter/pyspark-notebook

USER root
# Add essential packages
RUN apt-get update
RUN apt-get install -y build-essential nano apt-transport-https firefox gnupg curl libcurl4 libappindicator3-1
# PhantomJS
RUN conda install -c conda-forge phantomjs 
RUN phantomjs --version
# Chrome
RUN apt-get install -y libappindicator3-1
RUN wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
RUN sudo dpkg -i google-chrome-stable_current_amd64.deb
# Terraform
RUN wget https://releases.hashicorp.com/terraform/0.11.13/terraform_0.11.13_linux_amd64.zip
RUN unzip terraform_0.11.13_linux_amd64.zip
RUN mv terraform /usr/local/bin

# Set locale
RUN apt-get update && apt-get install -y locales \
    && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen \
    && locale-gen

# Install the MS ODBC SQL driver
# https://docs.microsoft.com/en-us/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server
RUN wget https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/msodbcsql_13.1.9.2-1_amd64.deb
RUN apt-get install -y libc6 libstdc++6 libkrb5-3 libcurl3 openssl debconf unixodbc unixodbc-dev
RUN ACCEPT_EULA=Y dpkg -i msodbcsql_13.1.9.2-1_amd64.deb

# Spark
RUN wget http://central.maven.org/maven2/org/apache/hadoop/hadoop-aws/2.7.3/hadoop-aws-2.7.3.jar -P $SPARK_HOME/jars/
RUN wget http://central.maven.org/maven2/com/amazonaws/aws-java-sdk/1.7.4/aws-java-sdk-1.7.4.jar -P $SPARK_HOME/jars/
RUN wget http://central.maven.org/maven2/org/apache/hadoop/hadoop-azure/2.7.3/hadoop-azure-2.7.3.jar -P $SPARK_HOME/jars/
RUN wget http://central.maven.org/maven2/com/microsoft/azure/azure-storage/6.1.0/azure-storage-6.1.0.jar -P $SPARK_HOME/jars/
RUN wget http://central.maven.org/maven2/com/microsoft/azure/azure-sqldb-spark/1.0.2/azure-sqldb-spark-1.0.2.jar -P $SPARK_HOME/jars/
RUN wget http://central.maven.org/maven2/com/microsoft/sqlserver/mssql-jdbc/7.0.0.jre8/mssql-jdbc-7.0.0.jre8.jar -P $SPARK_HOME/jars/
# Add password
COPY jupyter_notebook_config.py /home/jovyan/.jupyter/

USER $NB_USER
# Install geckodriver
RUN wget https://github.com/mozilla/geckodriver/releases/download/v0.23.0/geckodriver-v0.23.0-linux64.tar.gz 
RUN tar -zxvf geckodriver-v0.23.0-linux64.tar.gz
RUN pwd
# Install Python requirements
COPY requirements.txt /home/jovyan/
RUN pip install -r /home/jovyan/requirements.txt
# Install NLTK
# RUN python -c "import nltk; nltk.download('popular')"
# Custom styling
RUN mkdir -p /home/jovyan/.jupyter/custom
COPY custom.css /home/jovyan/.jupyter/custom/
# NB extensions
RUN jupyter contrib nbextension install --user
RUN jupyter nbextensions_configurator enable --user

