idf_component_register(SRCS "web_server.c"
                        INCLUDE_DIRS "include"
                        REQUIRES esp_http_server esp_timer esp_wifi esp_event nvs_flash esp_hw_support
                        EMBED_TXTFILES "webpage/index.html")
