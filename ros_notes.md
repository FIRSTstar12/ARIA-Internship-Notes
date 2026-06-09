# ROS notes

## How to change the ID number

This is the code to change the ID number of your ros domain. It **can't** be bigger than 232. Replace 10 with your chosen ID.

```bash
echo "export ROS_DOMAIN_ID=10" >> ~/.bashrc
source ~/.bashrc
```