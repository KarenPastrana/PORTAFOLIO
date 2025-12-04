# Actividad 8: Captura y Visualización de Video en Tiempo Real con OpenCV

```cpp
import cv2
 
 
video = cv2.VideoCapture(0)
 
while True:
    ret, frame = video.read()
    if not ret:
        break
    dibujo = frame.copy()
    cv2.line(dibujo, )
    print(" ")
    print(frame.shape[0])
    print(frame.shape[1])
 
    #mostrar img
    cv2.imshow("Video", frame)
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
...

