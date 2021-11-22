# spring-gumball ci/cd example
Just a repository to test CI/CD for my CMPE 172 lab. All code is authored by Professor Paul Nguyen. All documentation of process can be found in the README.md in my lab repository under Lab10.

## Submission
* We were asked to submit a link to our personal "public" GitHub Repo for Lab #10, which is this.
* I have also linked my class lab repository <a href='https://github.com/nguyensjsu/cmpe172-justin-zhu1018'>here</a> and the lab10 folder <a href='https://github.com/nguyensjsu/cmpe172-justin-zhu1018/tree/main/labs/lab10'>here</a> for ease of access to my README.md. Note that it is a private repository, since it is for the class only.
* I will also include my lab notes here for ease of access.

# CMPE 172 - Lab #10 Notes
Use the provided starter code from Professor Nguyen <a href='https://github.com/paulnguyen/cmpe172/tree/main/labs/lab10/starter-code/spring-gumball'>here</a> and create a personal repo named spring-gumball. We will then use it to work through the lab. The personal repo can be found here <a href='https://github.com/justin-zhu1018/spring-gumball'>justin-zhu1018/spring-gumball</a>.

## CI Workflow
1. Create a new Workflow with template `Java with Gradle` under Actions tab. It is no longer there, so I am just going to use the code snippet provided by Professor Nguyen. This is essentially the manual way of doing it.
![CI-1](https://user-images.githubusercontent.com/25803515/142794808-f9ad4dd7-edb2-478c-b616-6c6e481c0cc9.png)

2. Commit the code to the `main` branch to trigger the action. Take screenshots of the result after it runs.
![CI-2](https://user-images.githubusercontent.com/25803515/142794811-f1929489-4483-49d5-9572-e77fd0377648.png)
![CI-3](https://user-images.githubusercontent.com/25803515/142794815-f8eb1885-e274-45b3-8e49-9d86d3792026.png)
![CI-4](https://user-images.githubusercontent.com/25803515/142794824-c27d3b4e-6d96-482c-87ed-8ecdd9ee8812.png)
![CI-5](https://user-images.githubusercontent.com/25803515/142794828-76482ee1-6f73-4cbe-88fe-ed769cc1f718.png)
![CI-6](https://user-images.githubusercontent.com/25803515/142794832-6e3b14f3-93bf-4621-b9c7-f511c9f957c8.png)

## CD Workflow
1. Create a new Workflow build with template 'Build and Deploy to GKE' under Actions tab. I will once again be using a code snippet provided by Professor Nguyen for the sake of accuracy to ensure that I can get the lab running properly.
![CD-2](https://user-images.githubusercontent.com/25803515/142934693-f140a1bb-b8b1-46fa-9e14-56f6c18d20e6.png)

2. Reduce number of replicas from 4 to 2 in deployment.yaml. Downsizing as requested by Professor Nguyen to hopefully make things run smoother on GKE (was using too many resources). Also, ensure `kustomization.yml`, `deployment.yaml`, `service.yaml` are in the repo as well.
![CD-1](https://user-images.githubusercontent.com/25803515/142934650-b71bf6f8-d933-40bf-b236-a2411d859f7b.png)

3. Create a GKE cluster with name: `cmpe172` and region: `us-central1-c`. Size will be default to maximize resources.
![CD-6](https://user-images.githubusercontent.com/25803515/142934793-3de95731-f229-40d7-a67d-8f1b1c285bdd.png)

4. Set up secrets in the workspace. `GKE_PROJECT` with name of project and `GKE_SA_KEY` with Base64 encoded JSON service account key.
    * To create service account, go to IAM & Admin tab in GCP side nav, then to Service Accounts, Create Service Account with spring-gumball in required fields.
    * Then click on the created account and go to the Keys tab to create a new JSON key.
    * The service account needs to have owner access to the project.
![CD-3](https://user-images.githubusercontent.com/25803515/142934758-af84e558-210a-4708-a2e0-526ac4f88797.png)
![CD-4](https://user-images.githubusercontent.com/25803515/142934765-9cc4c9bb-de0d-4937-a120-00da64e8ad0e.png)
![CD-5](https://user-images.githubusercontent.com/25803515/142934780-26cfdcf3-13b7-496a-8100-f2f6ab4a5f77.png)

5. Go to GitHub Settings and Secrets tab to create secrets `GKE_PROJECT` and `GKE_SA_KEY` for GitHub to access them. `GKE_PROJECT` is project id and `GKE_SA_KEY` is the key generated in Step 3. The `GKE_SA_KEY` must be encoded with Base64 first. Do this by changing directory to the .json file downloaded and running `key_file_name.json | base64`.
![CD-A-9](https://user-images.githubusercontent.com/25803515/142935166-a2474533-fe63-4aed-b4f6-49f9758722c1.png)

6. We will now need to trigger the CD deployment by creating a new GitHub Release. I will be using version 1.0 tag. This should automatically deploy our app to our GCP GKE cluster. Focus on release 1.1.2. I had errors in prior versions that I ended up fixing after redoing the secrets part.
![CD-7](https://user-images.githubusercontent.com/25803515/142934954-c3cf498e-ed80-4eee-bac1-51cf8d0de23c.png)
![CD-8](https://user-images.githubusercontent.com/25803515/142934955-9d912829-e948-46ac-81dc-e722f6253529.png)
![CD-9-1](https://user-images.githubusercontent.com/25803515/142934965-26697656-d129-4c5b-9989-ffc7c18e9e5f.png)
![CD-9-2](https://user-images.githubusercontent.com/25803515/142934971-e68203e8-319b-4a5a-ac62-ca891c5d0002.png)

7. Manually create load balancer. Go to Services & Ingress, select the service, Create Ingress, External HTTP/S load balancer, spring-gumball-lb, make sure backend is selected, and everything else should be default, then click create.
![CD-A-1](https://user-images.githubusercontent.com/25803515/142935361-9c69cc88-e13b-46b8-8c60-904eb283fd0e.png)
![CD-A-2](https://user-images.githubusercontent.com/25803515/142935369-1ed7ac48-9573-414f-ac7e-b9eb260a5499.png)
![CD-A-3](https://user-images.githubusercontent.com/25803515/142935374-9b7de774-46e5-4fb1-92a6-c289e67b3e16.png)
![CD-A-4](https://user-images.githubusercontent.com/25803515/142935380-db74a373-49c5-4dca-ada7-070c4c7d9724.png)
![CD-A-5](https://user-images.githubusercontent.com/25803515/142935386-d155a762-9eaa-40c4-ac4e-9d06758d4626.png)
![CD-A-6](https://user-images.githubusercontent.com/25803515/142935387-6a0101c1-36aa-46a4-aef4-83d2e9446de9.png)
![CD-A-7](https://user-images.githubusercontent.com/25803515/142935397-872301ab-479b-42e5-8d83-2c47b1bf594c.png)

8. Test the page by going to the frontend created by the load balancer after the load balancer is finished creating.
![CD-A-8](https://user-images.githubusercontent.com/25803515/142935409-5668a97d-8b3a-4371-9f48-d4207a81dd40.png)
