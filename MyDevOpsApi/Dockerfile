# STAGE 1: Dùng bản SDK đầy đủ để restore và build code
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Tách riêng bước copy csproj để tận dụng bộ nhớ đệm (Docker layer cache)
COPY ["MyDevOpsApi.csproj", "./"]
RUN dotnet restore "MyDevOpsApi.csproj"

# Copy toàn bộ code còn lại vào và tiến hành publish
COPY . .
RUN dotnet publish "MyDevOpsApi.csproj" -c Release -o /app/publish /p:UseAppHost=false

# STAGE 2: Chỉ lấy bản Runtime siêu nhẹ để chạy, vứt bỏ toàn bộ SDK build
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=build /app/publish .

# Mở cổng mặc định của .NET 8
EXPOSE 8080
ENV ASPNETCORE_HTTP_PORTS=8080

ENTRYPOINT ["dotnet", "MyDevOpsApi.dll"]