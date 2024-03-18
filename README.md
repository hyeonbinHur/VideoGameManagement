# java-fx-video-game-management-service


## Contents
- [Overview](#overview)
- [Admin mode](#admin-mode)
- [Customer mode](#customer-mode)

## Overview

This project aims to create an online video game rental program using Java and JavaFX. The implementation involves two distinct modes: the admin mode and the customer mode, mirroring the functionalities of a real program. The admin mode primarily serves to manage both the video inventory and customer information. On the other hand, the customer mode focuses on facilitating the rental and return processes for video games.

To ensure the project's full functionality, the utilization of a database is imperative. However, for the purpose of managing the database server, several text files have been employed.

## Admin mode

https://github.com/hyeonBin7201/java-fx-video-game-management-service/assets/152157238/01771740-cd58-4e92-9e3a-ef09b22accfb

## Custoemr mode

https://github.com/hyeonBin7201/java-fx-video-game-management-service/assets/152157238/6e8f4675-0bd6-42b9-8bf5-1aeb7b3d5483

## Key functions

https://github.com/hyeonBin7201/java-fx-video-game-management-service/assets/152157238/6b69282e-77b1-4d60-b8dd-cecb2309b5bd

The key functions in this project is CRUD functions. The example of the CRUD fundtions are provided below

The example of the create function is that

```
public static boolean addUser(Customer newUser) {
        if(!checkIdFormat2(newUser.getId())) return false;

        boolean exist = false;
        for(int i = 0; i < userList.size(); i++){
            if(checkIdExist2(newUser.getId(), userList.get(i).getId())){
                exist = true;
                break;
            }
        }

        if(exist == false){
            userList.add(newUser);
            System.out.println("Add new user success!");
            saveFile();
            return true;
        }

        return false;
    }
```

The example of the delete function is that

```
public static boolean removeUser(String id){
        for(int i = 0; i < userList.size(); i++){
            if(id.equalsIgnoreCase(userList.get(i).getId())){
                userList.remove(i);
                System.out.println("Remove success!");
                saveFile();
                return true;
            }
        }
        System.out.println("User is not exist in the list");
        return false;
    }
```

the example of the update function is that

```
 public static boolean updateUser(Customer updateUser){
        for(int i = 0; i < userList.size(); i++){
            if(updateUser.getId().equalsIgnoreCase(userList.get(i).getId())){
                userList.get(i).updating(updateUser);
                System.out.println("Update success!");
                saveFile();
                return true;
            }
        }
        System.out.println("User is not existed!");
        return false;
    }
```

The example function for reading the txt file 

```
public static void readFile(){
        File myFile = new File(user_file_name);

        try {
            Scanner readLine = new Scanner(myFile);
            String line = readLine.nextLine();

            //Find the first customer information
            String[] customerInfo = line.split(",");
            while(readLine.hasNextLine() && customerInfo.length != 8) {
                line = readLine.nextLine();
                customerInfo = line.split(",");
            }

            //If there's no customer information in the file
            if(customerInfo.length != 8) return;

            while(readLine.hasNextLine()){
                customerInfo = line.split(",");

                line = readLine.nextLine();
                ArrayList <String> listRental = new ArrayList<>();

                //Read the list item until the next customer
                while(readLine.hasNextLine() && line.split(",").length == 1){
                    listRental.add(line);
                    line = readLine.nextLine();
                }

                if(!readLine.hasNextLine() && line.split(",").length == 1){
                    listRental.add(line);
                }

                Customer user = readUser(customerInfo, listRental);
                userList.add(user);
            }

            readLine.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
```

The example function for saving the txt file

```
public static void saveFile(){
        File myFile = new File(user_file_name);
        //ID,Name,Address,Phone,Number of rentals,customer type, username, password, list of items
        try {
            FileWriter writeFile = new FileWriter(myFile);
            for(Customer user : userList){
                writeFile.write(user.getId() + ",");
                writeFile.write(user.getName() + ",");
                writeFile.write(user.getAddress() + ",");
                writeFile.write(user.getPhone() + ",");
                writeFile.write(Integer.toString(user.getNumOfRentals()) + ",");
                writeFile.write(user.getCustomerType() + ",");
                writeFile.write(user.getUsername() + ",");
                writeFile.write(user.getPassword() + "\n");
                if(user.getNumOfRentals() > 0) {
                    for (int i = 0; i < user.getListRental().size(); i++) {
                        writeFile.write(user.getListRental().get(i) + "\n");
                    }
                } else {
                    writeFile.write("\n");
                }

            }
            writeFile.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

