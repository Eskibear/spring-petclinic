FROM mcr.microsoft.com/java/jre:8u192-zulu-debian9 AS base
EXPOSE 8080
WORKDIR /app

FROM mcr.microsoft.com/java/maven:8u192-zulu-debian9 AS build
WORKDIR /usr/src/app
COPY pom.xml ./
RUN /usr/local/bin/mvn-entrypoint.sh \
    mvn package dependency:copy-dependencies -Dmaven.test.skip=true -Dcheckstyle.skip=true -Dmaven.javadoc.skip=true --fail-never
COPY . .
RUN mvn package dependency:copy-dependencies -Dmaven.test.skip=true -Dcheckstyle.skip=true -Dmaven.javadoc.skip=true

FROM base AS final
COPY --from=build /usr/src/app/target/*.jar ./
COPY --from=build /usr/src/app/target/dependency ./dependency
ENTRYPOINT ["java","-cp","./*:./dependency/*","org.springframework.samples.petclinic.PetClinicApplication"]