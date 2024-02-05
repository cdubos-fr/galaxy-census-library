ENV['VAGRANT_DEFAULT_PROVIDER'] = 'docker'
$USERNAME = "developer"
$PROJECT = "galaxy-census-library"

Vagrant.configure("2") do |config|
  config.ssh.username = $USERNAME
  config.vm.provider "docker" do |docker|
    docker.name = "#{$PROJECT}-development"
    docker.build_dir = "docker"
    docker.remains_running = true
    docker.has_ssh = true
    docker.privileged = true
    docker.build_args = ["--target=vagrant"]
    docker.create_args = [
      "--workdir", "/home/#{$USERNAME}/#{$PROJECT}",
      "--mount", "type=bind,source=.,target=/home/#{$USERNAME}/#{$PROJECT}",
      "--hostname=#{$PROJECT}-devenv",
    ]
  end
  config.vm.synced_folder ".", "/home/#{$USERNAME}/#{$PROJECT}", disabled: true
  config.vm.synced_folder "./.venv", "/home/#{$USERNAME}/#{$PROJECT}/.venv", disabled: true
  config.vm.synced_folder "./.tox", "/home/#{$USERNAME}/#{$PROJECT}/.tox", disabled: true
  config.vm.synced_folder "./.ruff_cache", "/home/#{$USERNAME}/#{$PROJECT}/.ruff_cache", disabled: true

  config.vm.provision "shell",
    path: "./docker/provision.sh",
    args: [$USERNAME, $PROJECT],
    privileged: false
end
