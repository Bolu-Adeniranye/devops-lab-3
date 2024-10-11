Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"
  
  # Forward port 5000 from the guest (VM) to the host machine
  config.vm.network "forwarded_port", guest: 5000, host: 8080

  # Provision the VM using a shell script
  config.vm.provision "shell", inline: <<-SHELL
    # Update package list and install necessary packages
    sudo apt update
    sudo apt install -y git nano vim python-is-python3 python3-venv python3-pip
    
    # Set up the Python environment and Flask
    python -m venv /home/vagrant/flask_venv
    source /home/vagrant/flask_venv/bin/activate
    pip install Flask
  SHELL

  # Upload the hello.py file to the VM
  config.vm.provision "file", source: "hello.py", destination: "/home/vagrant/hello.py"

  # Run Flask automatically
  config.vm.provision "shell", inline: <<-SHELL
    source /home/vagrant/flask_venv/bin/activate
    nohup flask --app /home/vagrant/hello run --host=0.0.0.0 &
  SHELL
end
