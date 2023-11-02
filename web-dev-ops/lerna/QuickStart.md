```sh
# Create an empty directory  
mkdir ./new-lerna-workspace  
# Change into the new directory  
cd ./new-lerna-workspace  
# Initialize lerna (using --dryRun to preview the changes)  
npx lerna init --dryRun

# =>>> 新建package

lerna create package-name -y

# =>>> 构建

# 构建全部
npx lerna run build

# 构建指定package
npx lerna run build --scope=package-name



```