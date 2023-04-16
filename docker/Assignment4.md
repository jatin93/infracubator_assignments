# 1. Create a volume my_volume
       docker volume create my_volume
# 2. Create container and attach it to my_volume
        docker run -it -v my_volume:/bin alpine sh

# 3. Change something in volume folder, eg add a file with some content
## Change something in volume insdie the continer
    cd /bin (since in step no 2, -it is used hence the user is inside the container)
    touch fileInsideContainer.txt
## Chnge something in volume on local 
	docker inspect my_volume (take the mount path)
	colima ssh
	sudo su
	cd /var/lib/docker/volumes/my_volume/_data (mount path)
	touch fileOutsideContainer.txt
# 4. Create second container mounted with the same volume, check if it exists?
	docker run -it -v my_volume:/bin alpine sh
	cd /bin
	all contents of my_volume visible under the /bin in the alpine container(changes added from inside the conatainer and outside the container directly on volume path as well)