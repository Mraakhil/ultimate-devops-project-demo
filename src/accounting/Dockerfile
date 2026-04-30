# 1. Removed --platform=${BUILDPLATFORM}
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS builder

# 2. Set a default ARCH so the dotnet commands don't fail if TARGETARCH is empty
ARG TARGETARCH=amd64
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# ... (SDK image and ARGs) ...


# Remove the leading slash so it looks at the root of the context
COPY ["src/accounting/", "Accounting/"]
COPY ["pb/demo.proto", "Accounting/proto/"]

# ... (rest of the file) ...

# 3. Restoring using the defined TARGETARCH
RUN dotnet restore "./Accounting/Accounting.csproj" -r linux-$TARGETARCH
WORKDIR "/src/Accounting"

RUN dotnet build "./Accounting.csproj" -r linux-$TARGETARCH -c $BUILD_CONFIGURATION -o /app/build

# -----------------------------------------------------------------------------

FROM builder AS publish
ARG TARGETARCH=amd64
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Accounting.csproj" -r linux-$TARGETARCH -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# -----------------------------------------------------------------------------

FROM mcr.microsoft.com/dotnet/aspnet:8.0
USER app
WORKDIR /app
COPY --from=publish /app/publish .

# Standard setup for OpenTelemetry logs
USER root
RUN mkdir -p "/var/log/opentelemetry/dotnet"
RUN chown app "/var/log/opentelemetry/dotnet"

# Note: Ensure instrument.sh is actually in your /app/publish folder 
# or copy it specifically if it's missing.
RUN chmod +x ./instrument.sh && chown app "./instrument.sh"
USER app

ENTRYPOINT ["./instrument.sh", "dotnet", "Accounting.dll"]
