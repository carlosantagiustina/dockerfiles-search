FROM saagie/shiny4saagie

MAINTAINER Saagie

RUN apt-get update

RUN apt-get install -y --no-install-recommends -t unstable \
	bwidget \ 
	ca-certificates \ 
	curl \
	gdal-bin \
	git \
	gsl-bin \ 
	libcurl4-openssl-dev \ 
	libgdal-dev \ 
	libgeos-dev \ 
	libgeos++-dev \ 
	libproj-dev \ 
	libspatialite-dev \ 
	libv8-dev \ 
	libxml2-dev \ 
	netcdf-bin \ 
	pandoc pandoc-citeproc \ 
	qpdf \ 
	r-cran-rgl \ 
	r-cran-tkrplot \ 
	xauth \ 
	xfonts-base \ 
	xvfb \ 
	&& apt-get clean \ 
	&& rm -rf /var/lib/apt/lists/* \ 
	&& rm -rf /var/lib/apt/lists

RUN install2.r devtools 

## Install R spatial packages & cleanup 
RUN xvfb-run -a install.r \ 
	geoR \ 
	ggmap \ 
	ggvis \
	gstat \ 
	mapdata \
	maps \ 
	maptools \
	plotKML \
	RandomFields \
	rgdal \
	rgeos \
	shapefiles \
	sp \ 
	spatstat \
	raster \
	rasterVis \
	rts \
	&& installGithub.r s-u/fastshp \
	&& rm -rf /tmp/downloaded_packages/ /tmp/*.rds

RUN echo 'install.packages(c("shinydashboard", "rgdal", \
	"Cairo", "lubridate", "dplyr", "tidyr",  \
	"leaflet", "DT", "ggmap"), \
	repos="https://https://cloud.r-project.org")' > /tmp/packages.R \
	&& Rscript /tmp/packages.R
