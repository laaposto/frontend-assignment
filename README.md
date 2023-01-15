# FrontEnd Assignment
Assignment for Front-End position.

The goal of this assignment is to create a webpage that will help users investigate if an image is manipulated (deepfake) or not.

To complete the assignment, you will need to create a user interface that allows end-users to input an image URL and get the analysis results received from a back-end service.

The back-end service is already implemented and you must use it to build the front-end.

You must:

1.  Create a form where the user can enter the image URL
2.  Create an event listener that triggers the fetch of the image analysis results from the back-end service
3.  Display the analysis results in an appropriate format

You can use any front-end framework and technologies you are familiar with.

The back-end service offers the following endpoints:

1. POST `/jobs?url={image_url}`
   Sample response:
```json
{
"id":"23d9c543-500e-4d9c-a445-7c03970e8f93"
}
```

2. GET `/jobs/{job_id}`
   Sample response:
```json
{
"status":"COMPLETED",
"itemHash":"23d9c543-500e-4d9c-a445-7c03970e8f93"
}
```
3. GET `/reports/{job_id}`
   Sample response:
```json
{
  "hash": "cb43d420ed3f50ac1f9b70c4314ebc27",
  "deepfake_image_report": {
    "completed": true,
    "gpu": true,
    "image_path": "https://mever.iti.gr/deepfake/imgs/image1.jpg",
    "info": [
      {
        "bbox": {
          "left": 0.66,
          "top": 0.22,
          "right": 0.76,
          "bottom": 0.46
        },
        "prediction": 0.850284218788147
      },
      {
        "bbox": {
          "left": 0.27,
          "top": 0.22,
          "right": 0.37,
          "bottom": 0.46
        },
        "prediction": 0.21864309906959534
      }
    ],
    "message": "Process completed successfully.",
    "prediction": 0.850284218788147,
    "synthetic_prediction": 0.00006632010627072304,
    "prediction_time": 1.0709991719340906,
    "result": "FAKE"
  }
}
```
The API is served from the following URL: https://mever.iti.gr/deepfake/api/v3/images

To get the image analysis results from the back-end service you must follow the workflow presented below:

- Call `/jobs?url={image_url}` passing the image URL from the UI form as a parameter.
  Example : `/josbs?url=https://mever.iti.gr/deepfake/imgs/image1.jpg`

- Call `/jobs/{job_id}` with the `json.id` you received from the previous call
  Example : `/josbs/23d9c543-500e-4d9c-a445-7c03970e8f93`
  Check the `status` field of the response. If the status is `"PROCESSING"`, wait 1 second and call the endpoint again. Repeat this step until the status is `"COMPLETED"`.

- Call `/reports/{job_id}` with the `json.id` you received from the first call.
  Example : `/reports/23d9c543-500e-4d9c-a445-7c03970e8f93`

Parse the response and display the values of `message`, `prediction`, `synthetic_prediction`, `prediction_time`, and `result` to the user.

Moreover, the `info` array holds information any areas in the submitted image that might be manipulated. Each entry in the array is a bounding box with a `prediction` score. The bounding box coordinates are presented in relative position (in percentage) regarding the dimensions of the sumbitted image The top left corner of the bouding box is extracted from `bbox.top` and  `bbox.left` while the bottom right corner from `bbox.bottom` and `bbox.right`

Example on an 400x800 image with the following analysis result.
```json
 {
        "bbox": {
          "left": 0.27,
          "top": 0.22,
          "right": 0.37,
          "bottom": 0.46
        },
        "prediction": 0.21864309906959534
      }
```

Top left corner of this boundig box is at `left: 400*0.27` and `top:800*0.22` while the bottom right corner is at `right: 400*0.37` and `bottom:800*0.46` where the coordinates `0,0` are the top left corner of the submitted image.

The UI must present all bounding boxes and their prediction scores.

The provided URL must be checked to see if it is an image before submitting user input to the back-end service.

It is also important to make sure that the UI is responsive and looks good on different screen sizes.

To deliver the assignment, you should create a pull request to this repository that includes all necessary files such as HTML, CSS, and JS along with any comments you might have.

In the `images` folder  you can find images to test the UI.

Good luck!