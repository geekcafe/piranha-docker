ARG project_name
ARG project_path
ARG git_repo

# ------------- runtime layer  --------------------------------------------------------------------
# define our runtime layer first (keeps our layers down) 
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1.2 as runtime
WORKDIR /app
# ------------- / runtime layer  ------------------------------------------------------------------


# ------------- build layer  ----------------------------------------------------------------------
# create the build environment
FROM mcr.microsoft.com/dotnet/core/sdk:3.1 as build
# new layer so we need to reload the args
ARG project_name
ARG project_path
ARG git_repo

WORKDIR /src

RUN echo "cloning ${git_repo}"
# pull from git vs loading from our own file system
RUN git clone ${git_repo}

# only build the RazorWeb
RUN echo "swithing to working director: /src/${project_path}"
WORKDIR /src/${project_path}

# dotnet restore, build and publish
RUN dotnet restore "./"
RUN dotnet publish -c Release -o /build/out
RUN dotnet publish -c Release -o /build/publish
# ------------- / build layer  --------------------------------------------------------------------


# ------------- runtime/deploy  -------------------------------------------------------------------
FROM runtime as deploy

# new layer so we need to reload the args
ARG project_name
ARG project_path

WORKDIR /app
# copy from the build layer
COPY --from=build /build/publish .
# need the seed files or it blows up during the seeding process
COPY --from=build "/src/${project_path}/seed" ./seed
ENV ASPNETCORE_URLS http://*:5000


# use the arg info to create the entry point
# since the arg is gone by the time the entrypoint is executed this hack should do the trick
RUN echo "dotnet ${project_name}.dll" > "/app/launch.sh"
RUN chmod +x launch.sh
ENTRYPOINT ["sh", "/app/launch.sh"]



# ------------- usage  ----------------------------------------------------------------------------
## docker build --build-arg project_name=abc

# ------------- trouble shooting the build process  -----------------------------------------------
## if a build layer fails, you can "enter" that build layer and inspect the current contents
## docker run -it [image_id] sh
