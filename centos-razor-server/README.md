Create a storage container before running this:

  docker run --name razor-repo -v /var/lib/razor/repo-store -v /var/lib/pgsql -v /var/logs tianon/true

Start centos-razor-server and attach storage and link ports.

  docker run --name razor-server --hostname razor-server -d --volumes-from razor-repo -p 8080:8080 sedlund/centos-razor-server

You will need a DHCP/TFTP server to point to this container.  As well as a
razor-client installed somewhere to interact with the container.
