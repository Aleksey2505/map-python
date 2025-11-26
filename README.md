# Блок-схема приложения детекции людей

```mermaid
flowchart TD
    A[Запуск приложения PySide6 + QSettings] --> B{Режим работы?}
    
    B --> C1[Видео + SRT]
    B --> C2[Папка с фото]
    
    C1 --> V[Открыть видео cv2 + SRTParser]
    C2 --> P[Поиск фото glob + piexif GPS]
    
    V --> E[YOLO детекция conf/iou class 0 = человек]
    P --> E
    
    E --> F{Найден человек?}
    F -->|Да| G[Рисуем боксы save_frame_with_gps + piexif GPS]
    F -->|Нет| H[Пропустить или на карту если чекбокс]
    
    G --> I[Сохраняем результаты в data для Excel в map_data для карты]
    H --> I
    
    I --> J[Прогресс-бар QApplication.processEvents]
    J --> K{Конец цикла?}
    K -->|Нет| E
    K -->|Да| L[Генерация отчётов]
    
    L --> M{Есть кадры с людьми?}
    M -->|Да| N[Excel: frames_metadata.xlsx автопереименование при ошибке]
    M -->|Нет| O[Excel не создан]
    
    L --> Q{Есть точки для карты?}
    Q -->|Да| R[folium карта Esri.WorldImagery MarkerCluster + base64 превью]
    Q -->|Нет| S[Карта не создана]
    
    R --> T[Открыть в браузере]
    N --> U[status_label: ГОТОВО!]
    R --> U
    
    style A fill:#1e40af,color:white
    style E fill:#dc2626,color:white
    style R fill:#059669,color:white
    style U fill:#7c3aed,color:white,font-weight:bold
