#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["src/ApiGateways/GeekTime.Mobile.Gateway/GeekTime.Mobile.Gateway.csproj", "src/ApiGateways/GeekTime.Mobile.Gateway/"]
COPY ["NuGet.config","NuGet.config"]
RUN dotnet restore "src/ApiGateways/GeekTime.Mobile.Gateway/GeekTime.Mobile.Gateway.csproj"
COPY . .
WORKDIR "/src/src/ApiGateways/GeekTime.Mobile.Gateway"
RUN dotnet build "GeekTime.Mobile.Gateway.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "GeekTime.Mobile.Gateway.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
# RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime  #需要时设置时区
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "GeekTime.Mobile.Gateway.dll"]