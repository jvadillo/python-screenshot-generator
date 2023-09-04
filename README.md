# Python screenshot generator

Here's a step-by-step guide on how to create a Python script using the Playwright library and Cloudinary API to take screenshots from a list of URLs and store them in Cloudinary:

### Step 1: Install required libraries

First, install the necessary libraries for this project. You will need `playwright`, which is used to automate browser interactions, and `cloudinary` which is used to upload images to Cloudinary. You can install these libraries using pip command:

`pip install playwright cloudinary` 

### Step 2: Set up Cloudinary account

Next, sign up for a free Cloudinary account if you haven't already done so. Once you have an account, you will be able to access your Cloud name, API key, and API secret. These credentials will be needed to authenticate with Cloudinary.

### Step 3: Create a new Supabase table

Create a new table in your Supabase instance where you want to store the URL of the stored screenshot. This table should have two columns - one for the URL and another for the image ID returned by Cloudinary after successful upload.

### Step 4: Define the function to capture screenshots

Define a function called `capture_screenshot()` that accepts a URL as input parameter. Inside this function, use the `Playwright` library to launch a headless Chrome browser, navigate to the given URL, and capture a screenshot of the page. The screenshot should be saved locally as a PNG file.

    import os
    from playwright import sync_playwright
    
    def capture_screenshot(url):
        # Launch headless Chrome browser
        with sync_playwright() as p:
            browser = p.chromium.launch(headless=True)
            page = browser.new_page()
            
            # Navigate to the URL
            page.goto(url)
            
            # Capture screenshot
            screenshot_path = f"{os.getcwd()}/screenshot_a.png"
            page.screenshot(path=screenshot_path)
            
            return screenshot_path

### Step 5: Upload captured screenshot to Cloudinary
Inside the same function, use the `Cloudinary` library to upload the captured screenshot to Cloudinary. After successful upload, get the publicly accessible URL of the uploaded image and update it in the Supabase table created earlier.

    import cloudinary
    
    # Initialize Cloudinary client
    client = cloudinary.init(cloud_name="YOUR_CLOUD_NAME", api_key="YOUR_API_KEY", api_secret="YOUR_API_SECRET")
    
    # Upload screenshot to Cloudinary
    image_id = client.uploader().upload(screenshot_path).public_id
    
    # Update Supabase table with the uploaded image URL
    supa_table = supabase.Table("your_table_name")
    supa_table.update({ "image_url": image_id }).execute()` 

### Step 6: Call the function for each URL in the list

Finally, call the `capture_screenshot()` function for each URL in the list. Make sure to pass the URL as argument to the function.


    urls = ["https://www.example1.com", "https://www.example2.com"]
    for url in urls:
        capture_screenshot(url)

That's it! With these steps, you now have a fully functional Python script that captures screenshots from a list of URLs using Playwright and stores them in Cloudinary.
