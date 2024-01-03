# Testing1
Testing1
from fastapi import FastAPI, HTTPException
from fastapi.responses import StreamingResponse
import httpx

app = FastAPI()

@app.post("/stream-data")  # Changed to POST
async def stream_data():
    async with httpx.AsyncClient() as client:
        headers = {
            "Authorization": "Bearer YOUR_TOKEN_HERE"  # Replace with your actual token or method of authentication
        }
        
        # Replace with the actual data required by the API, if any
        data = {
            "key1": "value1",
            "key2": "value2"
        }
        
        try:
            # Changed to client.post and added json=data
            response = await client.post("http://first-api-endpoint/stream", headers=headers, json=data)

            # Ensure the request was successful
            response.raise_for_status()

            # Stream the response directly to the client
            return StreamingResponse(response.aiter_raw(), media_type="text/plain")

        except httpx.HTTPError as e:
            raise HTTPException(status_code=e.response.status_code, detail=str(e))
