# Lab 09: Knowledge check

**Answer each question below it, in your own words, in one to three sentences.** Replace the `...` below **Your answer:** with your text.

## 1. Credentials in the image?

Assume you want to share your image with others and make it publicly available on `ghcr.io`, so that anyone can pull it and run the analysis. A teammate suggests adding `COPY .aws /home/app/.aws` to the `Dockerfile`, so that nobody has to type `-v ~/.aws:/home/app/.aws:ro` any more. (This is a thought experiment: keep your own package private.)

**Your answer:** Give two reasons why this is a bad idea. Think of who could then read your credentials, and of what happens when your Learner Lab session ends.

...KNOWLEDGE_CHECK.md

The teammate's suggestion would be a security risk since the individual credential is availible for others to access, moreover, the credentials expire within a certain duration, so the image would no longer work after.

## 2. Where did the Atlas password come from?

In Step 4, you ran the analysis once with `-v ~/.aws:/home/app/.aws:ro` only, without the three `-e` options. The container still wrote to your Atlas cluster.

*Hint:* Read `get_atlas_settings()` in `src/book_analysis/analyze.py`.

**Your answer:** Explain, step by step, where the container found the Atlas settings in that run.

1. Container accesses my AWS Learner Lab credentials 
2. Not found, so it uses boto3 to request the mongodb_credentials from AWS SSM 
3. AWS returns the stored MONGODB credentials from ~/.aws
4. Using the credentials, the containers is able to connect to the Atlas 

## 3. Same tag, different image

Run `docker images book-analysis`, and note the image ID of `book-analysis:0.1.0`. Then change the `description` in the `LABEL` of your `Dockerfile`, build again with the same command, `docker build -t book-analysis:0.1.0 .`, and run `docker images book-analysis` again.

**Your answer:** Paste the image ID from before and after the rebuild. Why is it a problem for someone who uses your image that `0.1.0` now names a different image, and what should you have done instead?

Before: 77b38ca156fe
After: bd52c1df99d3

The image wouldn't be reproducible between two individuals because, even though they have the same tag, the image ID is different. This results in the tag not being able to identify the image and run it. Instead, 0.1.0 could be kept unchanged, and the modified image can be under a new tag, for instance 0.1.1.  
