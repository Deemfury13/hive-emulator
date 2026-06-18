# Етап 1: Збірка додатка в контейнері SDK
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build-env
WORKDIR /app

# Копіюємо весь вміст папки src в контейнер
COPY . ./

# Відновлюємо залежності та збираємо саме наш API-проєкт
RUN dotnet restore "CommunicationControl/DevOpsProject/DevOpsProject.CommunicationControl.API.csproj"
RUN dotnet publish "CommunicationControl/DevOpsProject/DevOpsProject.CommunicationControl.API.csproj" -c Release -o out

# Етап 2: Запуск релізу на мінімальному образі ASP.NET
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build-env /app/out .

# Налаштування користувача для безпеки
RUN groupadd --gid 1000 appuser && \
    useradd --uid 1000 --gid appuser --shell /bin/bash --create-home appuser
USER appuser

EXPOSE 8080
ENTRYPOINT ["dotnet", "DevOpsProject.CommunicationControl.API.dll"]