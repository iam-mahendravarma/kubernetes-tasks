I created 2 file one for pod and one for service while i run mysql file i got crashloopbackup then i checked in the internet by default mysql need root password while deploying so i add env for the root password now using temp mysql-client connect and check kubectl run mysql-client --rm -it --image=mysql8:0 -- bash after inter the root password i can able to access the db 

