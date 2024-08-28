FROM eclipse-temurin:17-jre

ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en'

COPY --chown=185 build/lib/QuoteCandleService-0.0.1-SNAPSHOT.jar app.jar

EXPOSE 9990
USER 185

ENTRYPOINT java $JAVA_OPTIONS -jar /app.jar

