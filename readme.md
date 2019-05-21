# Instructions

arrancar as VM com o vagrant (ansible, host1, host2):
  vagrant up

entrar na VM do ansible
  vagrant ssh ansible

mudar para o diretorio do vagrant
  cd /vagrant

testar o ansible
  ansible host1 -m ping
  ansible host2 -m ping
