# Pull official Nginx image
FROM nginx:alpine 

# Remove default nginx static assets
RUN rm -rf /usr/share/nginx/html/*   

# Copy custom static assets
COPY Devops_Intern.html /usr/share/nginx/html/index.html 

# Expose Nginx default port
EXPOSE 80  
                                                 
# Run Nginx in foreground
CMD ["nginx", "-g", "daemon off;"]                          
