# 导入jar包到本地仓库
mvn install:install-file -DgroupId=com.oracle "-DartifactId=ojbdc8" "-Dversion=8.0.1" "-Dpackaging=jar" "-Dfile=D:\ojdbc8.jar"