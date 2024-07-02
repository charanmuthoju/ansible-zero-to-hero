#Delete all existing keys in local/control node 
```
cd /.ssh/
rm -f id_rsa  id_rsa.pub
```

#To create key in local/control node
```
ssh-keygen
```
#Delete all existing keys in remote server
```
rm -f ~/.ssh/authorized_keys
```
#Enable password authentication
- Go to the file /etc/ssh/sshd_config.d/60-cloudimg-settings.conf
- Update PasswordAuthentication yes
- Restart SSH -> sudo systemctl restart ssh

#Add password to ubuntu user
```
sudo passwd ubuntu
```
#Copy ssh key to remote server
```
ssh-copy-id -i ~/.ssh/id_rsa.pub ubuntu@13.233.99.58
```
#Connect to remote server (-v used for debug mode)
```
ssh -v <Remote IP>
```
#Execute playbook 
```
ansible-playbook error_handling.yaml -i inventory.ini --vault-password-file vault.pass
```
#We can consider few specific errors by using failed.when module
```
- name: Check if a file exists in temp and fail task if it does
  ansible.builtin.command: ls /tmp/this_should_not_be_here
  register: result
  failed_when:
    - result.rc == 0
    - '"No such" not in result.stdout'
```
