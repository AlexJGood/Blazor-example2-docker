FROM nginx:stable-alpine-perl AS base


WORKDIR /app


EXPOSE 80


FROM microsoft/dotnet@sha256:2cb90bf0a775178967c58473ce5b5689de30221b9c4264b22f35d5e111e01d0a AS build

# Change working directory inside the image to /app
WORKDIR /src

COPY "./Blazor-example2.Client/" "Blazor-example2.Client/"

COPY "./Blazor-example2.Shared/" "Blazor-example2.Shared/"

# Run `dotnet restore` on the main web project file
RUN dotnet restore "Blazor-example2.Client/Blazor-example2.Client.csproj"



WORKDIR "/src/Blazor-example2.Client"
# Build the project in release mode
RUN dotnet build "Blazor-example2.Client.csproj" -c Release -o /app


FROM base AS final

COPY --from=build /src/Blazor-example2.Client/wwwroot /usr/share/nginx/html
COPY --from=build /src/Blazor-example2.Client/appdist /usr/share/nginx/html

COPY --from=build /src/Blazor-example2.Client/nginx.conf /etc/nginx/nginx.conf

ENV API_URL=http://blazor-server:9001/

