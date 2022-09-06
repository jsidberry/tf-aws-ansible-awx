# Installing AWX on EC2

This Terraform code will install an AWS EC2 with AWX, ready to be used.

# What is AWX
AWX is the Open-Sourced foundation of Ansible Tower. 

# Procedures:

This procedure will install AWX. It will create objects on AWS. You still have to `ssh` into the EC2 and run scripts (which are numbered, 1.x, 2,x, 3.x, 4.x).

It should not take more than 10 minutes. Before you start, go to the AWS console and create a AWS Key and Secret, which you will need to enter in the "override.tf" file before running the script. TODO: automate authentication.

⚠️ The Security Group rules configured by this script are wide open because this is for a POC. TODO: tighten down security in main.tf.


```
1) vi overfide.tf and put in your AWS access-keys and secret keys and awx desired password

          variable "access_key" { default = "put_access_key_value_here" }
          variable "secret_key" { default = "put_secret_key_value_here" }

          variable "awx_pass" { default = "putPassword_here" }

2) run the terraform script:
      a) terraform init
      b) terraform validate
      c) terraform apply

3) the output on the screen will give you the ec2 Public IP.  SSH to the ec2 with ec2-user@publicIP and run the following scripts:
     ./1.runansible_play.sh  # this ansible playbook will take 5 minutes to run
     ./2.install_galaxy_awx_task.sh
     ./3.install_galaxy-awx_web.sh
     ./4.create_aci_dir.sh
     TODO: automate this

4) Now you can browse to your awx ui.   Point browser to http://public_IP of ec2.   
     a) you can do a terraform output from the terraform workspace directory (where you ran the terraform plan from) to see the public ip
     b) the username will be admin and the password will be the one you specified in the overide.tf file
     TODO: automate this

5)  To Destroy:
     for full AWS constructs this script built:  terraform destroy
     to just destroy the ec2 only:   terraform destroy --target aws_instance.dummy-phy-ec2



```
