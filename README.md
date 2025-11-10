## AI-based-Classroom-Attendance-from-Face-Recognition
## Aim :
To build a system that automatically marks classroom attendance using AI-based face recognition, reducing manual roll call time and improving accuracy.

## System Requirements :
Input: Live camera feed or uploaded classroom photo.

Output: Attendance list with Present/Absent status for each student.

Storage: Database to maintain student profiles and attendance logs.

Interface: Web dashboard for teachers to view, verify, and export attendance reports.

## Features:
Detects faces in classroom images

Matches detected faces with known student images

Automatically marks attendance as Present/Absent

Simple CSV and visual output

## Program :
```
Developed by :k.pujitha
Register no : 212223240074
Date : 10-11-2025
!pip install cmake
!pip install dlib
!pip install face_recognition
!pip install opencv-python numpy

import cv2
import face_recognition
import os
import csv
from datetime import datetime


# ============================
# SETTINGS
# ============================
KNOWN_FACES_DIR = "students"       # Folder with known student images
ATTENDANCE_FILE = "attendance.csv" # Output CSV
INPUT_IMAGE = "classroom.jpg"      # If not using webcam, place image here
USE_WEBCAM = False                 # Set True for live camera

# ============================
# LOAD KNOWN STUDENT FACES
# ============================
print("[INFO] Loading known student faces...")
known_faces = []
known_names = []

for filename in os.listdir(KNOWN_FACES_DIR):
    if filename.endswith((".jpg", ".png")):
        path = os.path.join(KNOWN_FACES_DIR, filename)
        img = face_recognition.load_image_file(path)
        encodings = face_recognition.face_encodings(img)
        if encodings:
            known_faces.append(encodings[0])
            name = os.path.splitext(filename)[0]
            known_names.append(name)

print(f"[INFO] Loaded {len(known_names)} students.")

# ============================
# CAPTURE INPUT IMAGE
# ============================
if USE_WEBCAM:
    print("[INFO] Opening webcam... Press 'q' to capture frame.")
    cap = cv2.VideoCapture(0)
    while True:
        ret, frame = cap.read()
        cv2.imshow("Webcam - Press 'q' to capture", frame)
        if cv2.waitKey(1) & 0xFF == ord('q'):
            captured_frame = frame.copy()
            break
    cap.release()
    cv2.destroyAllWindows()
else:
    captured_frame = cv2.imread(INPUT_IMAGE)
    if captured_frame is None:
        raise FileNotFoundError(f"Image '{INPUT_IMAGE}' not found!")

# ============================
# DETECT & RECOGNIZE FACES
# ============================
print("[INFO] Detecting and recognizing faces...")

rgb_frame = cv2.cvtColor(captured_frame, cv2.COLOR_BGR2RGB)
face_locations = face_recognition.face_locations(rgb_frame)
face_encodings = face_recognition.face_encodings(rgb_frame, face_locations)

present_students = []
absent_students = known_names.copy()

for encoding, location in zip(face_encodings, face_locations):
    matches = face_recognition.compare_faces(known_faces, encoding)
    name = "Unknown"

    if True in matches:
        index = matches.index(True)
        name = known_names[index]
        if name not in present_students:
            present_students.append(name)
            if name in absent_students:
                absent_students.remove(name)

    # Draw rectangle and name
    top, right, bottom, left = location
    cv2.rectangle(captured_frame, (left, top), (right, bottom), (0, 255, 0), 2)
    cv2.putText(captured_frame, name, (left, top - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.8, (0, 255, 0), 2)

# ============================
# SAVE ATTENDANCE TO CSV
# ============================
print("[INFO] Saving attendance report...")

today = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
with open(ATTENDANCE_FILE, mode="w", newline="") as file:
    writer = csv.writer(file)
    writer.writerow(["Student Name", "Status", "Timestamp"])
    for name in known_names:
        status = "Present" if name in present_students else "Absent"
        writer.writerow([name, status, today])

print("[INFO] Attendance saved to", ATTENDANCE_FILE)

# ============================
# DISPLAY RESULTS
# ============================
print("\n========= ATTENDANCE SUMMARY =========")
for name in known_names:
    status = "✅ Present" if name in present_students else "❌ Absent"
    print(f"{name}: {status}")

cv2.imshow("Attendance Result", captured_frame)
cv2.waitKey(0)
cv2.destroyAll<img width="1038" height="696" alt="image" src="https://github.com/user-attachments/assets/f11fd28b-adaa-48a6-a8b9-bfc05dd2d18e" />
Windows()
```
## Output :
<img width="1042" height="696" alt="image" src="https://github.com/user-attachments/assets/2d940fc3-5628-4d9c-ad60-936420cefc70" />

<img width="624" height="257" alt="image" src="https://github.com/user-attachments/assets/b0ca0471-3de4-426a-aa32-127738c98204" />

## Result :

Thus , to build a system that automatically marks classroom attendance using AI-based face recognition, reducing manual roll call time and improving accuracy has been executed successfully.
