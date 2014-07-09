Create a storage container before running this

 docker run -v /var/lib/razor/repo-store -v /var/lib/pgsql --name razor-repo tianon/true

Start centos-razor-server and attach storage and link ports

 docker run -d --volumes-from razor-repo --name razor-server -p 69:69 -p 8080:8080 sedlund/centos-razor-server
