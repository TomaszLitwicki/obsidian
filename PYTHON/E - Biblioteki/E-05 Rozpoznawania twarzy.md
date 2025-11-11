opencv-python 4.12.0.88
`pip install opencv-python`

## Rozpoznawanie twarzy na zdjęciach
```Python
"""Rozpoznawanie twarzy na zdjęciach"""
import cv2.data

img = cv2.imread('tomasz01.jpg')
# print(img.shape) #wymiary obrazka (720, 1280, 3) kolor 3 skladowe
# print(img[0][0]) #kolor obrazka w danym miejscu 0.0

# Rozpoznawanie twarzy efektywne jest w kolorze B&W
gray_image = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

face_classifier = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')

face = face_classifier.detectMultiScale(gray_image, scaleFactor=1.2, minNeighbors=8, minSize=(50, 50))

print(face) #[181 211 124 124] współrzędne x1,y1,szerokość,wysokość

for (x,y,w,h) in face:
    cv2.rectangle(img, (x,y), (x+w, y+h), (0, 255, 0), 1)

cv2.imshow('tomasz',img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

## Rozpoznawanie twarzy na nagraniach live z kamery
```Python
"""Rozpoznawanie twarzy na obrazie wideo live"""
import cv2
import cv2.data

video = cv2.VideoCapture(0)

while True:
    success, frame = video.read()

    gray_image = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    face_classifier = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')

    face = face_classifier.detectMultiScale(gray_image, scaleFactor=1.2, minNeighbors=8, minSize=(100, 100))

    for (x, y, w, h) in face:
        cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 1)

    cv2.imshow('Video', frame)

    key = cv2.waitKey(1)
    if key != -1:
        break

video.release()
cv2.destroyAllWindows()
```

