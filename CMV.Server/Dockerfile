# Use uma imagem base menor e multi-stage para otimizar a imagem final
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /app

# Copie apenas os arquivos necessários para o build
COPY *.csproj ./
COPY src/CMV.Server/*.cs ./
RUN dotnet restore

# Compile a aplicação e copie apenas os arquivos necessários para o runtime
RUN dotnet publish -c Release -o /app/publish

# Use uma imagem base menor para o runtime
FROM mcr.microsoft.com/dotnet/aspnet:5.0-alpine
WORKDIR /app
COPY --from=build /app/publish .

# Exponha a porta e execute a aplicação
EXPOSE 80
ENTRYPOINT ["dotnet", "CMV.Server.dll"]
