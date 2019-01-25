FROM openjdk:8-jre-slim AS base
EXPOSE 8080
WORKDIR /app

FROM maven:3.5-jdk-8-slim AS build
WORKDIR /usr/src/app
COPY pom.xml ./
RUN /usr/local/bin/mvn-entrypoint.sh \
    mvn wro4j:run compile dependency:copy-dependencies -Dmaven.test.skip=true -Dcheckstyle.skip=true -Dmaven.javadoc.skip=true --fail-never
COPY . .
RUN mvn wro4j:run compile dependency:copy-dependencies -Dmaven.test.skip=true -Dcheckstyle.skip=true -Dmaven.javadoc.skip=true

FROM base AS final
COPY --from=build /usr/src/app/target/classes ./classes
COPY --from=build /usr/src/app/target/dependency ./dependency
ENTRYPOINT ["java","-cp","./classes:./dependency/*","org.springframework.samples.petclinic.PetClinicApplication"]