# Counting-cars
pip install ultralytics opencv-python
video_path = "/content/drive/MyDrive/car20.mp4"
import cv2
from ultralytics import solutions
video_path = "/content/drive/MyDrive/car20.mp4"
output_path = "/content/object_counting_output.avi"
cap = cv2.VideoCapture(video_path)
assert cap.isOpened(), "Error reading video file"
from google.colab import drive
drive.mount('/content/drive')
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
region_points = [(20, 400), (1080, 400), (1080, 360), (20, 360), (20, 400)]
video_writer = cv2.VideoWriter(output_path, cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))
counter = solutions.ObjectCounter(
    show=False,
    region=region_points,
)
while cap.isOpened():
    success, im0 = cap.read()
    if not success:
        print("Video processing completed.")
        break
    im0 = counter.count(im0)
    video_writer.write(im0)
cap.release()
video_writer.release()
cv2.destroyAllWindows()
from google.colab import files
files.download(output_path)
