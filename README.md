# Ansible Playbook Sample

# [Vagrant](https://www.vagrantup.com/)
Vagrant는 가상머신을 생성하고 관리하기 쉽게 도와준다. 인프라를 코드로써 관리하기 때문에 동일한 환경의 가상 머신을 사용할 수 있다.

1. Vagrantfile 작성
2. 가상 머신 구축
3. SSH로 가상 머신 접속

## Vagrant 명령어

### Vagrant 초기 파일 생성
생성된 Vagrantfile의 설정 값을 필요한 환경에 맞게 수정할 수 있다. 예를 들어 가상 머신 구축 후 필요한 패키지를 설치하는 스크립트를 실행할 수 있다.

```
vagrant init
```

### 가상 머신 실행
```
vagrant up
```

### 가상 머신 종료
```
vagrant halt
```

### 가상 머신 SSH 접속
```
vagrant ssh
```

### 가상 머신 삭제
```
vagrant destroy
```

## Vagrantfile 샘플 파일
아래는 Vagrantfile 샘플 파일이다.

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.hostname = "demo"
  config.vm.network "forwarded_port", guest: 80, host: 3000
  config.vm.synced_folder ".", "/home/vagrant/sync", disabled: true
  config.vm.provision "shell", inline: $script
end

$script = <<SCRIPT
  yum -y install epel-release
  yum -y install git
  yum -y install nginx
  echo "hello, vagrant" > /usr/share/nginx/html/index.html
  systemctl start nginx
SCRIPT
```

## Provision 적용
이미 가상 머신이 구축되고 나서 Provision을 설정한 경우에 이를 반영하기 위해서는 두 가지 방법이 있다.

1. 가상 머신을 삭제하고 새로 구축
2. <code>vagrant provision</code> 명령을 통해 지정된 부분만 다시 실행

</hr>

# [Ansible](https://www.ansible.com/)

## Playbook 실행
먼저 가상 머신에 SSH로 접속한다.
```
vagrant ssh
```

ansible 파일이 있는 저장소를 Clone 한다.

```
git clone https://github.com/junior-study/ansible-playbook-sample
```

Github에서 가져온 저장소로 이동해서 playbook을 실행한다.

```
cd ansible-playbook-sample
ansible-playbook -i development site.yml
```

<code>-i</code> 옵션은 사용할 특정 인벤토리 호스트를 명시하기 위해서 사용한다. 현재 예제에서는 development와 production 두 개로 구성되어 있다. 더 많은 옵션이 궁금하다면, [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html) 문서를 참고하자.