FROM maven:3.8.1-openjdk-17-slim AS builder
WORKDIR /deployServletProject
COPY . .
RUN mvn clean package -DskipTests

FROM tomcat:9.0.108-jdk17-corretto
RUN rm -rf /usr/local/tomcat/webapps/* //xóa mặc định của tomcat
COPY --from=builder /deployServletProject/target/DeployServletProject-1.0.war /usr/local/tomcat/webapps/ROOT.war
EXPOSE 8080
CMD ["catalina.sh", "run"]