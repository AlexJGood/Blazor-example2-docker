
FROM microsoft/dotnet:2.1.2-aspnetcore-runtime-alpine AS base


WORKDIR /app


EXPOSE 80


FROM microsoft/dotnet@sha256:2cb90bf0a775178967c58473ce5b5689de30221b9c4264b22f35d5e111e01d0a AS build

# Change working directory inside the image to /app
WORKDIR /src





COPY "./Blazor-example2.Server/" "Blazor-example2.Server/"

COPY "./Blazor-example2.Shared/" "Blazor-example2.Shared/"


RUN dotnet restore "Blazor-example2.Server/Blazor-example2.Server.csproj"



WORKDIR "/src/Blazor-example2.Server"
# Build the project in release mode
RUN dotnet build "Blazor-example2.Server.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "Blazor-example2.Server.csproj" -c Release -o /app



FROM base AS final
WORKDIR /app
COPY --from=publish /app .

ENTRYPOINT ["dotnet", "Blazor-example2.Server.dll"]