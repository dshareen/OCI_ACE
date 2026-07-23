Overview
--------

Deployment Steps
----------------
1. Navigate to Object Storage > Buckets
   Log in to the OCI console and open the navigation menu at the upper left corner. Scroll down until you find the "Storage" section. Under "Object Storage & Archive Storage", you can find "Buckets". Click on "Buckets".
   ![Figure 1](1.png)

2. Create a folder directory
   Open the destination target(Bucket you want to upload to). I have opened my bucket "CV_SCREEN_BUCKET" and, within the bucket management tab, click on the "Action" button, where you will see an option for "Create new folder"
   ![Figure 2](2.png)

   In this scenario, we are manually creating a folder here. But in the upcoming steps, we can implement it using the script as well(for that option, it is not necessary to create the folder manually here).

   ![Figure 3](3.png)

   Specify a meaningful name for the newly created folder.

   ![Figure 4](4.png)
   In here i have created multiple folders as per the requirement. Verify the newly created directories appear correctly within the object listing interface.

Note: Creating a "folder" establishes a string prefix attached to the front of a file's name (e.g., `Folder_Name/file.png`). The OCI console parses the forward slashes (`/`) visually to represent standard folders for human readability.

3. Configuring Manual Object Uploads
   ![Figure 5](5.png)
   Click "Upload object" to upload a file/object manually. Specify the namespace tier for naming the object and also select the "Storage tier" parameters.

   ![Figure 6](6.png)
   Expand the advanced upload settings to configure the "Additional checksum" validation profile (e.g. SHA-256) to ensure strict data integrity validation during ingestion.

   ![Figure 7](7.png)
   Drag and drop the file into the directed zone or manually upload the file by choosing from the pc's local storage.

   ![Figure 8](8.png)
   Expand the "Optional response headers and metadata" panel to inject custom key-value metadata tags or define strict HTTP headers.

   ![Figure 9](9.png)
   In this review stage, you can verify specs before uploading them to the bucket.

4. Initiating Pre-Authenticated Requests
   To securely authenticate external scripts or users without exposing IAM credentials, navigate to the "Resources" menu on the bucket overview page and select "Pre-authenticated requests".
   ![Figure 10](11.png)

5. Generating a PAR
   
   ![Figure 11](12.png)
   Click "Create pre-authenticated request" and configure the target scope. You can target the entire bucket or lock the PAR to specific objects.

   ![Figure 12](13.png)
   Define the access type according to the permission needed to be given and set a strict expiration date to make the request access time-bound.

   ![Figure 13](14.png)
   Once the pre-authentication request is completed, copy the "Pre-authentication URL" as it won't be displayed again after closing this window.
   
   ![Figure 14](15.png)
   To adhere to the principle of least privilege, generate prefix-specific PARs (e.g. scoped only to the `Senior Engineer...` folder). This ensures that a compromised PAR cannot interact with data outside its designated directory.

6. Automating Uploads via PowerShell & REST API

   With the PAR URL generated, automation scripts can interact directly with the OCI REST API. The script below targets a specific OCI folder prefix and uses the `Invoke-RestMethod` cmdlet to execute HTTP PUT requests, uploading local PDF files sequentially.
   ![Figure 15](16.png)

   The script handles URI encoding for spaces in filenames and outputs green success messages for every file successfully ingested into the cloud bucket.
   ![Figure 16](17.png)


7. Verifying Automated Uploads in the OCI Console
   After coming to the OCI bucket, we can see the uploaded objects listed in the storage bucket
   ![Figure 17](18.png)

   You can also go inside the folder to check whether the uploaded objects are listed within the mentioned folder.
   ![Figure 18](19.png)
   
