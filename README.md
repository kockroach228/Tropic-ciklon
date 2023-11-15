# Tropic-ciklon
import cv2
import numpy as np
import os

def detect_cyclones(image_path):
    # Загрузка изображения
    image = cv2.imread(image_path)

    # Преобразование изображения в оттенки серого
    gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

    # Применение фильтра Гаусса для сглаживания изображения
    blurred_image = cv2.GaussianBlur(gray_image, (5, 5), 0)

    # Применение алгоритма Хафа для обнаружения окружностей
    circles = cv2.HoughCircles(blurred_image, cv2.HOUGH_GRADIENT, 1, 50,
                               param1=50, param2=30, minRadius=10, maxRadius=100)

    # Создание .txt-файла для записи результатов
    result_path = image_path.replace('.jpg', '.txt')
    result_file = open(result_path, 'w')

    # Запись координат и радиусов обнаруженных окружностей в .txt-файл результатов
    if circles is not None:
        circles = np.round(circles[0, :]).astype("int")
        for (x, y, r) in circles:
            result_file.write(f"Cyclone: {x}, {y}, {r}\n")

    # Закрытие .txt-файла
    result_file.close()

# Обход всех изображений .jpg в текущем каталоге
image_files = [file for file in os.listdir() if file.endswith(".jpg")]
for image_file in image_files:
    detect_cyclones(image_file)



