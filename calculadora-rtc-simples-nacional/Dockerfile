FROM eclipse-temurin:25-jdk AS build
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends maven && rm -rf /var/lib/apt/lists/*
COPY calculadora-rtc-regime-regular/pom.xml ./pom.xml
COPY calculadora-rtc-regime-regular/flyway ./flyway
RUN mkdir -p ./calculadora/db \
    && mvn flyway:migrate -Dflyway.configFiles=./flyway/flyway-pro.conf --batch-mode --errors
COPY calculadora-rtc-simples-nacional/ .
RUN mvn clean package -DskipTests

FROM eclipse-temurin:25-jre
WORKDIR /app
COPY --from=build /app/target/api-simples-nacional.jar app.jar
COPY --from=build /app/calculadora ./calculadora
EXPOSE 8080
ENTRYPOINT ["java", "-Dspring.profiles.active=offline", "-Dserver.port=8080", "-jar", "app.jar"]