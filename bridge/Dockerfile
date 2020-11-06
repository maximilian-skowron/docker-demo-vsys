FROM nginx:latest

RUN apt-get update && apt-get install -y --no-install-recommends \
	curl \
	inetutils-ping \
	&& rm -rf /var/lib/apt/lists/*

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
