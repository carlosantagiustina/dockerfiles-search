#
# Provides simple HTTP to HTTPS redirect
#
# VERSION               2.0.1
#
FROM nginx:alpine

#
# Redirection HTTP code. 
# Use either 307 (default) - Temporary Redirect
# Or 308 - Permanent Redirect
#
ENV HTTPCODE 307

# Port to expose, this is currently fixed to 80
EXPOSE 80

# Makes only the entry point, no other points needed
RUN mkdir -p /entrypoint;

#
# Prepares the entrypoint primer script, and runs it once
#
RUN echo '#!/bin/sh'                                                                   > /entrypoint/primer.sh && \
	echo ''                                                                             >> /entrypoint/primer.sh && \
	echo '# Goes into the nginx config folder'                                          >> /entrypoint/primer.sh && \
	echo 'cd /etc/nginx/conf.d/'                                                        >> /entrypoint/primer.sh && \
	echo ''                                                                             >> /entrypoint/primer.sh && \
	echo '# Setup the server config'                                                    >> /entrypoint/primer.sh && \
	echo 'echo "server {"                                          > default.conf'      >> /entrypoint/primer.sh && \
	echo 'echo "   listen 80;"                                     >> default.conf'     >> /entrypoint/primer.sh && \
	echo 'echo "   return $HTTPCODE https://\$host\$request_uri;"  >> default.conf'     >> /entrypoint/primer.sh && \
	echo 'echo "}"                                                 >> default.conf'     >> /entrypoint/primer.sh && \
	echo ''                                                                             >> /entrypoint/primer.sh && \
	echo '# Goes back to root folder'                                                   >> /entrypoint/primer.sh && \
	echo 'cd /'                                                                         >> /entrypoint/primer.sh && \
	echo ''                                                                             >> /entrypoint/primer.sh && \
	echo '# Chain the execution commands'                                               >> /entrypoint/primer.sh && \
	echo 'exec "$@"'                                                                    >> /entrypoint/primer.sh && \
	chmod +x /entrypoint/primer.sh && \
	/entrypoint/primer.sh;

#
# Useful for debugging
#
# RUN cat /etc/nginx/conf.d/default.conf;
# RUN cat /entrypoint/primer.sh;

ENTRYPOINT ["/entrypoint/primer.sh"]
CMD nginx -g "daemon off;"
