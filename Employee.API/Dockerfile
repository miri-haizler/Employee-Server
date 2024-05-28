#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Employee.API/Employee.API.csproj", "Employee.API/"]
COPY ["Employee.Data/Employee.Data.csproj", "Employee.Data/"]
COPY ["Employee.Core/Employee.Core.csproj", "Employee.Core/"]
COPY ["Employee.Service/Employee.Service.csproj", "Employee.Service/"]
RUN dotnet restore "Employee.API/Employee.API.csproj"
COPY . .
WORKDIR "/src/Employee.API"
RUN dotnet build "Employee.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Employee.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Employee.API.dll"]