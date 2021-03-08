resource "digitalocean_droplet" "myc2-1" {
    image = "ubuntu-20-04-x64"
    name = "myc2-1"
    region = "sfo2"
    size = "s-1vcpu-1gb"
    private_networking = true
    ssh_keys = [
      data.digitalocean_ssh_key.keyname.id
    ]

connection {
    host = self.ipv4_address
    user = "root"
    type = "ssh"
    private_key = file(var.pvt_key)
    timeout = "2m"
  }

provisioner "remote-exec" {
    inline = [
				"sudo apt-get update -y",
				"sudo apt-get -y install curl",
				"sudo apt-get -y install ufw",
				"sudo ufw default allow outgoing",
				"sudo ufw allow from 127.0.0.1 to any port 22",
				"sudo ufw allow from 127.0.0.1 to any port 80",
				"sudo ufw allow from 127.0.0.1 to any port 443",
				"sudo ufw --force enable",
				"echo \"deb http://repo.pritunl.com/stable/apt focal main\" | tee /etc/apt/sources.list.d/pritunl.list",
				"echo \"deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse\" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list",
				"curl -fsSL https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -",
				"sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com --recv 7568D9BB55FF9E5287D586017AE645C0CF8E292A",
				"sudo apt-get update -y",
				"apt --assume-yes install pritunl mongodb-server",
				"systemctl start pritunl mongodb",
				"systemctl enable pritunl mongodb",
    ]
  }
}
