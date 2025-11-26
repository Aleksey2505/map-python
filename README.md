flowchart TD
    A[Запуск приложения<br>PySide6 + QSettings] --> B{Режим работы?}
    
    B -->|Видео + SRT| V[Открыть видео<br>cv2 + SRTParser]
    B -->|Папка с фото| P[Поиск фото<br>glob + piexif GPS]
    
    V --> E[YOLO детекция<br>conf/iou<br>class 0 = человек]
    P --> E
    
    E --> F{Найден человек?}
    F -->|Да| G[Рисуем боксы<br>save_frame_with_gps<br>+ piexif GPS]
    F -->|Нет| H[Пропустить<br>или на карту если чекбокс]
    
    G --> I[Сохраняем результаты<br>в data для Excel<br>в map_data для карты]
    H --> I
    
    I --> J[Прогресс-бар<br>QApplication.processEvents]
    J --> K{Конец цикла?}
    K -->|Нет| E
    K -->|Да| L[Генерация отчётов]
    
    L --> M{Есть кадры с людьми?}
    M -->|Да| N[Excel: frames_metadata.xlsx<br>автопереименование при ошибке]
    M -->|Нет| O[Excel не создан]
    
    L --> Q{Есть точки для карты?}
    Q -->|Да| R[folium карта<br>Esri.WorldImagery<br>MarkerCluster + base64 превью]
    Q -->|Нет| S[Карта не создана]
    
    R --> T[Открыть в браузере]
    N --> U[status_label: ГОТОВО!]
    R --> U
    
    style A fill:#1e40af,color:white
    style E fill:#dc2626,color:white
    style R fill:#059669,color:white
    style U fill:#7c3aed,color:white,font-weight:bold
