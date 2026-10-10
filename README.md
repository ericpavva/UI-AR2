--[[
    tetescr hub — UI v9.7
    Barra no topo + ordem customizada + retangular + idiomas
]]

-- ============================================================
-- CARREGA A LIBRARY NL
-- ============================================================
local NL
do
    local ok, lib = pcall(function()
        return loadstring(game:HttpGet("https://raw.githubusercontent.com/DeviceHB21/mylib/refs/heads/main/source.lua"))()
    end)
    if not ok or not lib then
        warn("[tetescr] Falha ao carregar NL: " .. tostring(lib))
        return
    end
    NL = lib
end

-- Esconde o ScreenGui inteiro durante a montagem (evita flash)
if NL.ScreenGui then
    NL.ScreenGui.Enabled = false
end

-- ============================================================
-- PALETA
-- ============================================================
local ACCENT_DEFAULT = Color3.fromRGB(60, 160, 255)
local ROW_BG       = Color3.fromRGB(28, 30, 34)
local ROW_BG_HOVER = Color3.fromRGB(45, 75, 130)
local BOX_OFF      = Color3.fromRGB(45, 48, 58)
local TEXT_OFF     = Color3.fromRGB(175, 178, 186)
local TEXT_ON      = Color3.fromRGB(240, 240, 245)
local TEXT_HOVER   = Color3.fromRGB(255, 255, 255)

pcall(function() if NL.SetAccentColor then NL:SetAccentColor(ACCENT_DEFAULT) end end)
NL.AccentColor = ACCENT_DEFAULT

local Tween     = game:GetService("TweenService")
local TextSvc   = game:GetService("TextService")
local UserInput = game:GetService("UserInputService")

-- ============================================================
-- SISTEMA DE IDIOMA
-- ============================================================
local LOCALES = {
    ["English"] = {
        tab_esp = "ESP", tab_combat = "Combat", tab_movement = "Movement",
        tab_cars = "Cars", tab_world = "World", tab_radar = "Radar",
        tab_teleport = "Teleport", tab_diversos = "Misc", tab_cos = "Cosmetics",
        tab_config = "Config",

        sec_player_visuals = "Player Visuals", sec_filters = "Filters",
        sec_aim = "Aim", sec_weapon = "Weapon",
        sec_movement_player = "Player", sec_fly = "Fly",
        sec_cars_tuning = "Tuning", sec_cars_noclip = "Noclip",
        sec_lighting = "Lighting", sec_radar_main = "Radar",
        sec_minimap = "Minimap", sec_tp_player = "Player TP",
        sec_freeze = "Freeze", sec_camera = "Camera",
        sec_unlocker = "Unlocker", sec_keybinds = "Keybinds",
        sec_window = "Window", sec_language = "Language",

        opt_enable_esp = "Enable ESP", opt_player_name = "Player Name",
        opt_player_box = "Player Box", opt_player_health = "Player Health",
        opt_player_distance = "Player Distance", opt_player_skeleton = "Player Skeleton",
        opt_player_chams = "Player Chams", opt_ignore_teammates = "Ignore Teammates",
        opt_visible_only = "Visible Only", opt_render_distance = "Render Distance",

        opt_silent_aim = "Silent Aim", opt_rage_bot = "Rage Bot",
        opt_wall_check = "Wall Check", opt_fov = "FOV",
        opt_smoothing = "Smoothing", opt_no_recoil = "No Recoil",
        opt_no_spread = "No Spread", opt_wallbang = "Wallbang",
        opt_hitmarker = "Hitmarker",

        opt_speed_hack = "Speed Hack", opt_speed_pct = "Speed %",
        opt_infinite_jump = "Infinite Jump", opt_noclip = "Noclip",
        opt_bunny_hop = "Bunny Hop", opt_fly = "Fly",
        opt_fly_speed = "Fly Speed", opt_car_fly = "Car Fly",

        opt_inf_fuel = "Infinite Fuel", opt_boost = "Boost",
        opt_godmode = "Godmode", opt_vehicle_noclip = "Vehicle Noclip",
        opt_noclip_speed = "Noclip Speed",

        opt_fullbright = "Fullbright", opt_no_fog = "No Fog",
        opt_brightness = "Brightness", opt_time = "Time",

        opt_radar_enabled = "Enabled", opt_radar_names = "Names",
        opt_radar_dist = "Distance", opt_minimap_size = "Size",

        opt_target = "Target", opt_behind = "Behind",
        opt_tp_to_target = "TP To Target", opt_open_map = "Open Map",

        opt_freeze_zombies = "Freeze Zombies", opt_whole_server = "Whole Server",
        opt_radius = "Radius", opt_fov_zoom = "FOV Zoom",
        opt_anti_aim = "Anti-Aim", opt_jump_circle = "Jump Circle",

        opt_enable_selection = "Enable Selection",
        opt_remove_all_cos = "Remove All Cosmetics",

        opt_click_row = "Click a row to set the key",
        opt_player_esp_kb = "Player ESP", opt_silent_aim_kb = "Silent Aim",
        opt_fly_kb = "Fly", opt_reset_kb = "Reset All Keybinds",
        opt_save_config = "Save Config", opt_load_config = "Load Config",
        opt_reset_defaults = "Reset Defaults",
    },

    ["Português"] = {
        tab_esp = "ESP", tab_combat = "Combate", tab_movement = "Movimento",
        tab_cars = "Veículos", tab_world = "Mundo", tab_radar = "Radar",
        tab_teleport = "Teleporte", tab_diversos = "Diversos",
        tab_cos = "Cosméticos", tab_config = "Config",

        sec_player_visuals = "Visual do Jogador", sec_filters = "Filtros",
        sec_aim = "Mira", sec_weapon = "Arma",
        sec_movement_player = "Jogador", sec_fly = "Voo",
        sec_cars_tuning = "Ajustes", sec_cars_noclip = "No Clip",
        sec_lighting = "Iluminação", sec_radar_main = "Radar",
        sec_minimap = "Minimapa", sec_tp_player = "TP Jogador",
        sec_freeze = "Congelar", sec_camera = "Câmera",
        sec_unlocker = "Desbloqueador", sec_keybinds = "Teclas",
        sec_window = "Janela", sec_language = "Idioma",

        opt_enable_esp = "Ativar ESP", opt_player_name = "Nome do Jogador",
        opt_player_box = "Caixa do Jogador", opt_player_health = "Vida do Jogador",
        opt_player_distance = "Distância do Jogador",
        opt_player_skeleton = "Esqueleto do Jogador",
        opt_player_chams = "Chams do Jogador", opt_ignore_teammates = "Ignorar Aliados",
        opt_visible_only = "Só Visíveis", opt_render_distance = "Distância de Renderização",

        opt_silent_aim = "Silent Aim", opt_rage_bot = "Rage Bot",
        opt_wall_check = "Checar Parede", opt_fov = "FOV",
        opt_smoothing = "Suavização", opt_no_recoil = "Sem Recuo",
        opt_no_spread = "Sem Dispersão", opt_wallbang = "Wallbang",
        opt_hitmarker = "Hitmarker",

        opt_speed_hack = "Velocidade", opt_speed_pct = "Velocidade %",
        opt_infinite_jump = "Pulo Infinito", opt_noclip = "Noclip",
        opt_bunny_hop = "Bunny Hop", opt_fly = "Voar",
        opt_fly_speed = "Velocidade do Voo", opt_car_fly = "Carro Voador",

        opt_inf_fuel = "Combustível Infinito", opt_boost = "Turbo",
        opt_godmode = "Deus", opt_vehicle_noclip = "Noclip do Veículo",
        opt_noclip_speed = "Velocidade do Noclip",

        opt_fullbright = "Iluminação Total", opt_no_fog = "Sem Névoa",
        opt_brightness = "Brilho", opt_time = "Hora",

        opt_radar_enabled = "Ativado", opt_radar_names = "Nomes",
        opt_radar_dist = "Distância", opt_minimap_size = "Tamanho",

        opt_target = "Alvo", opt_behind = "Atrás",
        opt_tp_to_target = "TP para o Alvo", opt_open_map = "Abrir Mapa",

        opt_freeze_zombies = "Congelar Zumbis", opt_whole_server = "Servidor Todo",
        opt_radius = "Raio", opt_fov_zoom = "Zoom de FOV",
        opt_anti_aim = "Anti-Aim", opt_jump_circle = "Círculo de Pulo",

        opt_enable_selection = "Ativar Seleção",
        opt_remove_all_cos = "Remover Todos os Cosméticos",

        opt_click_row = "Clique na linha para definir a tecla",
        opt_player_esp_kb = "ESP do Jogador", opt_silent_aim_kb = "Silent Aim",
        opt_fly_kb = "Voar", opt_reset_kb = "Resetar Todas as Teclas",
        opt_save_config = "Salvar Config", opt_load_config = "Carregar Config",
        opt_reset_defaults = "Resetar Padrões",
    },

    ["Español"] = {
        tab_esp = "ESP", tab_combat = "Combate", tab_movement = "Movimiento",
        tab_cars = "Vehículos", tab_world = "Mundo", tab_radar = "Radar",
        tab_teleport = "Teletransporte", tab_diversos = "Varios",
        tab_cos = "Cosméticos", tab_config = "Config",

        sec_player_visuals = "Visual del Jugador", sec_filters = "Filtros",
        sec_aim = "Apuntado", sec_weapon = "Arma",
        sec_movement_player = "Jugador", sec_fly = "Vuelo",
        sec_cars_tuning = "Ajustes", sec_cars_noclip = "No Clip",
        sec_lighting = "Iluminación", sec_radar_main = "Radar",
        sec_minimap = "Minimapa", sec_tp_player = "TP Jugador",
        sec_freeze = "Congelar", sec_camera = "Cámara",
        sec_unlocker = "Desbloqueador", sec_keybinds = "Teclas",
        sec_window = "Ventana", sec_language = "Idioma",

        opt_enable_esp = "Activar ESP", opt_player_name = "Nombre del Jugador",
        opt_player_box = "Caja del Jugador", opt_player_health = "Vida del Jugador",
        opt_player_distance = "Distancia del Jugador",
        opt_player_skeleton = "Esqueleto del Jugador",
        opt_player_chams = "Chams del Jugador", opt_ignore_teammates = "Ignorar Aliados",
        opt_visible_only = "Solo Visibles", opt_render_distance = "Distancia de Render",

        opt_silent_aim = "Silent Aim", opt_rage_bot = "Rage Bot",
        opt_wall_check = "Comprobar Pared", opt_fov = "FOV",
        opt_smoothing = "Suavizado", opt_no_recoil = "Sin Retroceso",
        opt_no_spread = "Sin Dispersión", opt_wallbang = "Wallbang",
        opt_hitmarker = "Hitmarker",

        opt_speed_hack = "Velocidad", opt_speed_pct = "Velocidad %",
        opt_infinite_jump = "Salto Infinito", opt_noclip = "Noclip",
        opt_bunny_hop = "Bunny Hop", opt_fly = "Volar",
        opt_fly_speed = "Velocidad de Vuelo", opt_car_fly = "Coche Volador",

        opt_inf_fuel = "Combustible Infinito", opt_boost = "Turbo",
        opt_godmode = "Modo Dios", opt_vehicle_noclip = "Noclip del Vehículo",
        opt_noclip_speed = "Velocidad del Noclip",

        opt_fullbright = "Luz Total", opt_no_fog = "Sin Niebla",
        opt_brightness = "Brillo", opt_time = "Hora",

        opt_radar_enabled = "Activado", opt_radar_names = "Nombres",
        opt_radar_dist = "Distancia", opt_minimap_size = "Tamaño",

        opt_target = "Objetivo", opt_behind = "Detrás",
        opt_tp_to_target = "TP al Objetivo", opt_open_map = "Abrir Mapa",

        opt_freeze_zombies = "Congelar Zombis", opt_whole_server = "Todo el Servidor",
        opt_radius = "Radio", opt_fov_zoom = "Zoom de FOV",
        opt_anti_aim = "Anti-Aim", opt_jump_circle = "Círculo de Salto",

        opt_enable_selection = "Activar Selección",
        opt_remove_all_cos = "Quitar Todos los Cosméticos",

        opt_click_row = "Haz clic en la fila para asignar tecla",
        opt_player_esp_kb = "ESP del Jugador", opt_silent_aim_kb = "Silent Aim",
        opt_fly_kb = "Volar", opt_reset_kb = "Reiniciar Todas las Teclas",
        opt_save_config = "Guardar Config", opt_load_config = "Cargar Config",
        opt_reset_defaults = "Restaurar Por Defecto",
    },

    ["Français"] = {
        tab_esp = "ESP", tab_combat = "Combat", tab_movement = "Mouvement",
        tab_cars = "Véhicules", tab_world = "Monde", tab_radar = "Radar",
        tab_teleport = "Téléportation", tab_diversos = "Divers",
        tab_cos = "Cosmétiques", tab_config = "Config",

        sec_player_visuals = "Visuel du Joueur", sec_filters = "Filtres",
        sec_aim = "Visée", sec_weapon = "Arme",
        sec_movement_player = "Joueur", sec_fly = "Vol",
        sec_cars_tuning = "Réglages", sec_cars_noclip = "No Clip",
        sec_lighting = "Éclairage", sec_radar_main = "Radar",
        sec_minimap = "Mini-carte", sec_tp_player = "TP Joueur",
        sec_freeze = "Geler", sec_camera = "Caméra",
        sec_unlocker = "Débloqueur", sec_keybinds = "Touches",
        sec_window = "Fenêtre", sec_language = "Langue",

        opt_enable_esp = "Activer ESP", opt_player_name = "Nom du Joueur",
        opt_player_box = "Boîte du Joueur", opt_player_health = "Vie du Joueur",
        opt_player_distance = "Distance du Joueur",
        opt_player_skeleton = "Squelette du Joueur",
        opt_player_chams = "Chams du Joueur", opt_ignore_teammates = "Ignorer Alliés",
        opt_visible_only = "Visibles Seulement", opt_render_distance = "Distance d'Affichage",

        opt_silent_aim = "Silent Aim", opt_rage_bot = "Rage Bot",
        opt_wall_check = "Vérifier Mur", opt_fov = "FOV",
        opt_smoothing = "Lissage", opt_no_recoil = "Sans Recul",
        opt_no_spread = "Sans Dispersion", opt_wallbang = "Wallbang",
        opt_hitmarker = "Hitmarker",

        opt_speed_hack = "Vitesse", opt_speed_pct = "Vitesse %",
        opt_infinite_jump = "Saut Infini", opt_noclip = "Noclip",
        opt_bunny_hop = "Bunny Hop", opt_fly = "Voler",
        opt_fly_speed = "Vitesse de Vol", opt_car_fly = "Voiture Volante",

        opt_inf_fuel = "Carburant Infini", opt_boost = "Turbo",
        opt_godmode = "Mode Dieu", opt_vehicle_noclip = "Noclip du Véhicule",
        opt_noclip_speed = "Vitesse Noclip",

        opt_fullbright = "Pleine Lumière", opt_no_fog = "Sans Brouillard",
        opt_brightness = "Luminosité", opt_time = "Heure",

        opt_radar_enabled = "Activé", opt_radar_names = "Noms",
        opt_radar_dist = "Distance", opt_minimap_size = "Taille",

        opt_target = "Cible", opt_behind = "Derrière",
        opt_tp_to_target = "TP vers la Cible", opt_open_map = "Ouvrir la Carte",

        opt_freeze_zombies = "Geler les Zombies", opt_whole_server = "Tout le Serveur",
        opt_radius = "Rayon", opt_fov_zoom = "Zoom FOV",
        opt_anti_aim = "Anti-Aim", opt_jump_circle = "Cercle de Saut",

        opt_enable_selection = "Activer la Sélection",
        opt_remove_all_cos = "Retirer Tous les Cosmétiques",

        opt_click_row = "Cliquez sur la ligne pour définir la touche",
        opt_player_esp_kb = "ESP du Joueur", opt_silent_aim_kb = "Silent Aim",
        opt_fly_kb = "Voler", opt_reset_kb = "Réinitialiser les Touches",
        opt_save_config = "Sauvegarder", opt_load_config = "Charger Config",
        opt_reset_defaults = "Réinitialiser",
    },

    ["Deutsch"] = {
        tab_esp = "ESP", tab_combat = "Kampf", tab_movement = "Bewegung",
        tab_cars = "Fahrzeuge", tab_world = "Welt", tab_radar = "Radar",
        tab_teleport = "Teleport", tab_diversos = "Verschiedenes",
        tab_cos = "Kosmetik", tab_config = "Config",

        sec_player_visuals = "Spieler-Anzeige", sec_filters = "Filter",
        sec_aim = "Zielen", sec_weapon = "Waffe",
        sec_movement_player = "Spieler", sec_fly = "Fliegen",
        sec_cars_tuning = "Feinabstimmung", sec_cars_noclip = "No Clip",
        sec_lighting = "Beleuchtung", sec_radar_main = "Radar",
        sec_minimap = "Minikarte", sec_tp_player = "TP Spieler",
        sec_freeze = "Einfrieren", sec_camera = "Kamera",
        sec_unlocker = "Freischalter", sec_keybinds = "Tasten",
        sec_window = "Fenster", sec_language = "Sprache",

        opt_enable_esp = "ESP Aktivieren", opt_player_name = "Spielername",
        opt_player_box = "Spieler-Box", opt_player_health = "Spieler-Leben",
        opt_player_distance = "Spieler-Distanz",
        opt_player_skeleton = "Spieler-Skelett",
        opt_player_chams = "Spieler-Chams", opt_ignore_teammates = "Teammates Ignorieren",
        opt_visible_only = "Nur Sichtbare", opt_render_distance = "Sichtweite",

        opt_silent_aim = "Silent Aim", opt_rage_bot = "Rage Bot",
        opt_wall_check = "Wand-Check", opt_fov = "FOV",
        opt_smoothing = "Glättung", opt_no_recoil = "Kein Rückstoß",
        opt_no_spread = "Keine Streuung", opt_wallbang = "Wallbang",
        opt_hitmarker = "Hitmarker",

        opt_speed_hack = "Geschwindigkeit", opt_speed_pct = "Geschw. %",
        opt_infinite_jump = "Unendlich Springen", opt_noclip = "Noclip",
        opt_bunny_hop = "Bunny Hop", opt_fly = "Fliegen",
        opt_fly_speed = "Fluggeschwindigkeit", opt_car_fly = "Auto Fliegen",

        opt_inf_fuel = "Unendl. Treibstoff", opt_boost = "Turbo",
        opt_godmode = "Gottmodus", opt_vehicle_noclip = "Fahrzeug Noclip",
        opt_noclip_speed = "Noclip-Geschwindigkeit",

        opt_fullbright = "Volle Helligkeit", opt_no_fog = "Kein Nebel",
        opt_brightness = "Helligkeit", opt_time = "Uhrzeit",

        opt_radar_enabled = "Aktiviert", opt_radar_names = "Namen",
        opt_radar_dist = "Distanz", opt_minimap_size = "Größe",

        opt_target = "Ziel", opt_behind = "Dahinter",
        opt_tp_to_target = "TP zum Ziel", opt_open_map = "Karte Öffnen",

        opt_freeze_zombies = "Zombies Einfrieren", opt_whole_server = "Ganzer Server",
        opt_radius = "Radius", opt_fov_zoom = "FOV-Zoom",
        opt_anti_aim = "Anti-Aim", opt_jump_circle = "Sprung-Kreis",

        opt_enable_selection = "Auswahl Aktivieren",
        opt_remove_all_cos = "Alle Kosmetik Entfernen",

        opt_click_row = "Klicke auf die Zeile, um Taste zu setzen",
        opt_player_esp_kb = "Spieler ESP", opt_silent_aim_kb = "Silent Aim",
        opt_fly_kb = "Fliegen", opt_reset_kb = "Alle Tasten Zurücksetzen",
        opt_save_config = "Speichern", opt_load_config = "Laden",
        opt_reset_defaults = "Standard Wiederherstellen",
    },

    ["Русский"] = {
        tab_esp = "ESP", tab_combat = "Бой", tab_movement = "Движение",
        tab_cars = "Машины", tab_world = "Мир", tab_radar = "Радар",
        tab_teleport = "Телепорт", tab_diversos = "Разное",
        tab_cos = "Косметика", tab_config = "Настройки",

        sec_player_visuals = "Отображение Игрока", sec_filters = "Фильтры",
        sec_aim = "Прицел", sec_weapon = "Оружие",
        sec_movement_player = "Игрок", sec_fly = "Полёт",
        sec_cars_tuning = "Настройка", sec_cars_noclip = "No Clip",
        sec_lighting = "Освещение", sec_radar_main = "Радар",
        sec_minimap = "Мини-карта", sec_tp_player = "TP Игрока",
        sec_freeze = "Заморозка", sec_camera = "Камера",
        sec_unlocker = "Разблокировка", sec_keybinds = "Клавиши",
        sec_window = "Окно", sec_language = "Язык",

        opt_enable_esp = "Включить ESP", opt_player_name = "Имя Игрока",
        opt_player_box = "Бокс Игрока", opt_player_health = "Здоровье",
        opt_player_distance = "Дистанция",
        opt_player_skeleton = "Скелет Игрока",
        opt_player_chams = "Chams Игрока", opt_ignore_teammates = "Игнорировать Союзников",
        opt_visible_only = "Только Видимые", opt_render_distance = "Дистанция Отрисовки",

        opt_silent_aim = "Silent Aim", opt_rage_bot = "Rage Bot",
        opt_wall_check = "Проверка Стены", opt_fov = "FOV",
        opt_smoothing = "Сглаживание", opt_no_recoil = "Без Отдачи",
        opt_no_spread = "Без Разброса", opt_wallbang = "Wallbang",
        opt_hitmarker = "Hitmarker",

        opt_speed_hack = "Скорость", opt_speed_pct = "Скорость %",
        opt_infinite_jump = "Бесконечный Прыжок", opt_noclip = "Noclip",
        opt_bunny_hop = "Bunny Hop", opt_fly = "Полёт",
        opt_fly_speed = "Скорость Полёта", opt_car_fly = "Летающая Машина",

        opt_inf_fuel = "Бесконечное Топливо", opt_boost = "Турбо",
        opt_godmode = "Бог", opt_vehicle_noclip = "Noclip Машины",
        opt_noclip_speed = "Скорость Noclip",

        opt_fullbright = "Яркое Освещение", opt_no_fog = "Без Тумана",
        opt_brightness = "Яркость", opt_time = "Время",

        opt_radar_enabled = "Включено", opt_radar_names = "Имена",
        opt_radar_dist = "Дистанция", opt_minimap_size = "Размер",

        opt_target = "Цель", opt_behind = "Сзади",
        opt_tp_to_target = "TP к Цели", opt_open_map = "Открыть Карту",

        opt_freeze_zombies = "Заморозить Зомби", opt_whole_server = "Весь Сервер",
        opt_radius = "Радиус", opt_fov_zoom = "FOV Зум",
        opt_anti_aim = "Anti-Aim", opt_jump_circle = "Круг Прыжка",

        opt_enable_selection = "Включить Выбор",
        opt_remove_all_cos = "Убрать Всю Косметику",

        opt_click_row = "Кликните по строке чтобы задать клавишу",
        opt_player_esp_kb = "ESP Игрока", opt_silent_aim_kb = "Silent Aim",
        opt_fly_kb = "Полёт", opt_reset_kb = "Сбросить Все Клавиши",
        opt_save_config = "Сохранить", opt_load_config = "Загрузить",
        opt_reset_defaults = "Сброс по Умолчанию",
    },

    ["中文"] = {
        tab_esp = "ESP", tab_combat = "战斗", tab_movement = "移动",
        tab_cars = "载具", tab_world = "世界", tab_radar = "雷达",
        tab_teleport = "传送", tab_diversos = "杂项",
        tab_cos = "外观", tab_config = "设置",

        sec_player_visuals = "玩家视觉", sec_filters = "过滤器",
        sec_aim = "瞄准", sec_weapon = "武器",
        sec_movement_player = "玩家", sec_fly = "飞行",
        sec_cars_tuning = "调校", sec_cars_noclip = "穿墙",
        sec_lighting = "光照", sec_radar_main = "雷达",
        sec_minimap = "小地图", sec_tp_player = "玩家传送",
        sec_freeze = "冻结", sec_camera = "相机",
        sec_unlocker = "解锁", sec_keybinds = "快捷键",
        sec_window = "窗口", sec_language = "语言",

        opt_enable_esp = "启用 ESP", opt_player_name = "玩家名称",
        opt_player_box = "玩家方框", opt_player_health = "玩家血量",
        opt_player_distance = "玩家距离",
        opt_player_skeleton = "玩家骨架",
        opt_player_chams = "玩家透视", opt_ignore_teammates = "忽略队友",
        opt_visible_only = "仅可见", opt_render_distance = "渲染距离",

        opt_silent_aim = "Silent Aim", opt_rage_bot = "Rage Bot",
        opt_wall_check = "墙壁检测", opt_fov = "FOV",
        opt_smoothing = "平滑度", opt_no_recoil = "无后坐力",
        opt_no_spread = "无扩散", opt_wallbang = "Wallbang",
        opt_hitmarker = "命中标记",

        opt_speed_hack = "加速", opt_speed_pct = "速度 %",
        opt_infinite_jump = "无限跳跃", opt_noclip = "穿墙",
        opt_bunny_hop = "连跳", opt_fly = "飞行",
        opt_fly_speed = "飞行速度", opt_car_fly = "飞行载具",

        opt_inf_fuel = "无限燃料", opt_boost = "加速",
        opt_godmode = "无敌", opt_vehicle_noclip = "载具穿墙",
        opt_noclip_speed = "穿墙速度",

        opt_fullbright = "全亮", opt_no_fog = "无雾",
        opt_brightness = "亮度", opt_time = "时间",

        opt_radar_enabled = "启用", opt_radar_names = "名称",
        opt_radar_dist = "距离", opt_minimap_size = "大小",

        opt_target = "目标", opt_behind = "后方",
        opt_tp_to_target = "传送到目标", opt_open_map = "打开地图",

        opt_freeze_zombies = "冻结僵尸", opt_whole_server = "全服务器",
        opt_radius = "范围", opt_fov_zoom = "FOV 缩放",
        opt_anti_aim = "Anti-Aim", opt_jump_circle = "跳跃圈",

        opt_enable_selection = "启用选择",
        opt_remove_all_cos = "移除所有外观",

        opt_click_row = "点击行以设置按键",
        opt_player_esp_kb = "玩家 ESP", opt_silent_aim_kb = "Silent Aim",
        opt_fly_kb = "飞行", opt_reset_kb = "重置所有按键",
        opt_save_config = "保存配置", opt_load_config = "加载配置",
        opt_reset_defaults = "重置默认",
    },
}

-- Mapa de chaves: cada objeto TextLabael registrado + chave de tradução
local TRANSLATABLE = {}
local CURRENT_LANG = "English"

local function registerTranslatable(obj, key)
    if not obj or not key then return end
    table.insert(TRANSLATABLE, { obj = obj, key = key })
end

local function t(key)
    local dict = LOCALES[CURRENT_LANG] or LOCALES["English"]
    return dict[key] or (LOCALES["English"][key] or key)
end

local function applyLocale(lang)
    if not LOCALES[lang] then lang = "English" end
    CURRENT_LANG = lang
    local dict = LOCALES[lang]
    for _, entry in ipairs(TRANSLATABLE) do
        local translated = dict[entry.key]
        if translated and entry.obj and entry.obj.Parent then
            pcall(function() entry.obj.Text = translated end)
        end
    end
end

-- Persistência: lê idioma salvo
local CONFIG_FOLDER = "tetescrHUB_Configs"
local LOCALE_FILE   = CONFIG_FOLDER .. "/locale.txt"

task.spawn(function()
    pcall(function()
        if isfolder and not isfolder(CONFIG_FOLDER) then
            if makefolder then makefolder(CONFIG_FOLDER) end
        end
    end)
    pcall(function()
        if isfile and isfile(LOCALE_FILE) then
            local saved = readfile(LOCALE_FILE)
            if saved and LOCALES[saved] then
                CURRENT_LANG = saved
            end
        end
    end)
end)

local function saveLocale(lang)
    pcall(function()
        if writefile then writefile(LOCALE_FILE, lang) end
    end)
end

-- ============================================================
-- HELPERS
-- ============================================================
local function iconFont()
    if NL.BuiltInBold then return NL.BuiltInBold end
    return Enum.Font.GothamBold
end

local function clearLabelChildren(labelFrame)
    for _, c in ipairs(labelFrame:GetChildren()) do
        if c:IsA("TextLabel") then
            c.Visible = false
        elseif c:IsA("Frame") then
            local hasLayout = false
            for _, sub in ipairs(c:GetChildren()) do
                if sub:IsA("UIListLayout") then hasLayout = true break end
            end
            if not hasLayout then c.Visible = false end
        end
    end
end

-- ============================================================
-- ROW
-- ============================================================
local function injectRow(labelFrame, config)
    config = config or {}
    local name     = config.Name or "Option"
    local key      = config.TransKey
    local default  = config.Default or false
    local keyVal   = config.Keybind
    local callback = config.Callback or function() end
    local accent   = config.Accent or ACCENT_DEFAULT

    clearLabelChildren(labelFrame)
    labelFrame.BackgroundColor3 = ROW_BG
    labelFrame.BackgroundTransparency = 1
    labelFrame.Size = UDim2.new(1, -10, 0, 28)
    labelFrame.ClipsDescendants = false

    local rowBg = Instance.new("Frame")
    rowBg.Name = "rowBg"
    rowBg.BackgroundColor3 = ROW_BG_HOVER
    rowBg.BackgroundTransparency = 1
    rowBg.BorderSizePixel = 0
    rowBg.Size = UDim2.fromScale(1, 1)
    rowBg.ZIndex = labelFrame.ZIndex + 1
    rowBg.Parent = labelFrame

    local box = Instance.new("Frame")
    box.Name = "box"
    box.BackgroundColor3 = default and accent or BOX_OFF
    box.BorderSizePixel = 0
    box.AnchorPoint = Vector2.new(0, 0.5)
    box.Position = UDim2.new(0, 12, 0.5, 0)
    box.Size = UDim2.fromOffset(14, 14)
    box.ZIndex = labelFrame.ZIndex + 10
    box.Parent = labelFrame

    local chk = Instance.new("TextLabel")
    chk.Name = "check"
    chk.BackgroundTransparency = 1
    chk.Size = UDim2.fromScale(1, 1)
    chk.FontFace = iconFont()
    chk.Text = "check"
    chk.TextColor3 = Color3.fromRGB(255, 255, 255)
    chk.TextSize = 12
    chk.TextTransparency = default and 0 or 1
    chk.ZIndex = box.ZIndex + 1
    chk.Parent = box

    local lbl = Instance.new("TextLabel")
    lbl.Name = "label"
    lbl.BackgroundTransparency = 1
    lbl.AnchorPoint = Vector2.new(0, 0.5)
    lbl.Position = UDim2.new(0, 34, 0.5, 0)
    lbl.Size = UDim2.new(1, -100, 1, 0)
    lbl.Font = Enum.Font.GothamMedium
    lbl.Text = key and t(key) or name
    lbl.TextColor3 = default and TEXT_ON or TEXT_OFF
    lbl.TextSize = 13
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.ZIndex = labelFrame.ZIndex + 10
    lbl.Parent = labelFrame

    if key then registerTranslatable(lbl, key) end

    local keyBtn
    if keyVal ~= nil then
        keyBtn = Instance.new("TextButton")
        keyBtn.Name = "keybind"
        keyBtn.BackgroundColor3 = Color3.fromRGB(45, 48, 58)
        keyBtn.BorderSizePixel = 0
        keyBtn.AnchorPoint = Vector2.new(1, 0.5)
        keyBtn.Position = UDim2.new(1, -10, 0.5, 0)
        keyBtn.Size = UDim2.fromOffset(40, 18)
        keyBtn.Font = Enum.Font.GothamMedium
        keyBtn.Text = tostring(keyVal):sub(1, 5)
        keyBtn.TextColor3 = Color3.fromRGB(220, 220, 230)
        keyBtn.TextSize = 11
        keyBtn.AutoButtonColor = false
        keyBtn.ZIndex = labelFrame.ZIndex + 12
        keyBtn.Parent = labelFrame

        local kc = Instance.new("UICorner")
        kc.CornerRadius = UDim.new(0, 3)
        kc.Parent = keyBtn

        local ks = Instance.new("UIStroke")
        ks.Color = accent
        ks.Thickness = 1.5
        ks.Transparency = 0.5
        ks.Parent = keyBtn
    end

    local state = { value = default, key = keyVal }

    local function paint()
        Tween:Create(box, TweenInfo.new(0.1), {
            BackgroundColor3 = state.value and accent or BOX_OFF,
        }):Play()
        Tween:Create(chk, TweenInfo.new(0.1), {
            TextTransparency = state.value and 0 or 1,
        }):Play()
        Tween:Create(lbl, TweenInfo.new(0.1), {
            TextColor3 = state.value and TEXT_ON or TEXT_OFF,
        }):Play()
    end

    local function toggle()
        state.value = not state.value
        paint()
        callback(state.value)
    end

    labelFrame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then
            toggle()
        end
    end)

    labelFrame.MouseEnter:Connect(function()
        Tween:Create(rowBg, TweenInfo.new(0.12), { BackgroundTransparency = 0.82 }):Play()
        Tween:Create(lbl, TweenInfo.new(0.12), { TextColor3 = TEXT_HOVER }):Play()
    end)
    labelFrame.MouseLeave:Connect(function()
        Tween:Create(rowBg, TweenInfo.new(0.12), { BackgroundTransparency = 1 }):Play()
        Tween:Create(lbl, TweenInfo.new(0.12), {
            TextColor3 = state.value and TEXT_ON or TEXT_OFF,
        }):Play()
    end)

    paint()

    local sep = Instance.new("Frame")
    sep.Name = "separator"
    sep.BackgroundColor3 = Color3.fromRGB(50, 52, 60)
    sep.BackgroundTransparency = 0.4
    sep.BorderSizePixel = 0
    sep.AnchorPoint = Vector2.new(0, 1)
    sep.Position = UDim2.new(0, 8, 1, 0)
    sep.Size = UDim2.new(1, -16, 0, 1)
    sep.ZIndex = labelFrame.ZIndex + 2
    sep.Parent = labelFrame

    return {
        Root = labelFrame,
        GetValue = function() return state.value end,
        SetValue = function(v) state.value = v paint() callback(v) end,
    }
end

-- ============================================================
-- SLIDER
-- ============================================================
local function injectSlider(labelFrame, config)
    config = config or {}
    local name     = config.Name or "Value"
    local key      = config.TransKey
    local min      = config.Min or 0
    local max      = config.Max or 100
    local default  = config.Default or min
    local rounding = config.Rounding or 0
    local callback = config.Callback or function() end
    local accent   = config.Accent or ACCENT_DEFAULT

    clearLabelChildren(labelFrame)
    labelFrame.BackgroundColor3 = ROW_BG
    labelFrame.BackgroundTransparency = 1
    labelFrame.Size = UDim2.new(1, -10, 0, 42)

    local lbl = Instance.new("TextLabel")
    lbl.BackgroundTransparency = 1
    lbl.Position = UDim2.new(0, 12, 0, 4)
    lbl.Size = UDim2.new(1, -80, 0, 14)
    lbl.Font = Enum.Font.GothamMedium
    lbl.Text = key and t(key) or name
    lbl.TextColor3 = TEXT_OFF
    lbl.TextSize = 13
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.ZIndex = labelFrame.ZIndex + 10
    lbl.Parent = labelFrame

    if key then registerTranslatable(lbl, key) end

    local val = Instance.new("TextLabel")
    val.BackgroundTransparency = 1
    val.AnchorPoint = Vector2.new(1, 0)
    val.Position = UDim2.new(1, -12, 0, 4)
    val.Size = UDim2.new(0, 70, 0, 14)
    val.Font = Enum.Font.GothamBold
    val.Text = tostring(default)
    val.TextColor3 = accent
    val.TextSize = 13
    val.TextXAlignment = Enum.TextXAlignment.Right
    val.ZIndex = labelFrame.ZIndex + 10
    val.Parent = labelFrame

    local bar = Instance.new("Frame")
    bar.Name = "bar"
    bar.BackgroundColor3 = Color3.fromRGB(35, 38, 44)
    bar.BorderSizePixel = 0
    bar.Position = UDim2.new(0, 12, 0, 26)
    bar.Size = UDim2.new(1, -24, 0, 6)
    bar.ZIndex = labelFrame.ZIndex + 10
    bar.Parent = labelFrame

    local barCorner = Instance.new("UICorner")
    barCorner.CornerRadius = UDim.new(1, 0)
    barCorner.Parent = bar

    local barStroke = Instance.new("UIStroke")
    barStroke.Color = accent
    barStroke.Thickness = 1
    barStroke.Transparency = 0.65
    barStroke.Parent = bar

    local fill = Instance.new("Frame")
    fill.Name = "fill"
    fill.BackgroundColor3 = accent
    fill.BorderSizePixel = 0
    fill.Size = UDim2.fromScale((default - min) / math.max(max - min, 1), 1)
    fill.ZIndex = labelFrame.ZIndex + 11
    fill.Parent = bar

    local fillCorner = Instance.new("UICorner")
    fillCorner.CornerRadius = UDim.new(1, 0)
    fillCorner.Parent = fill

    local state = { value = default }

    local function setFromX(px)
        local abs, sz = bar.AbsolutePosition, bar.AbsoluteSize
        local pct = math.clamp((px - abs.X) / math.max(sz.X, 1), 0, 1)
        local raw = min + (max - min) * pct
        local mult = 10 ^ rounding
        local rounded = math.floor(raw * mult + 0.5) / mult
        state.value = rounded
        val.Text = tostring(rounded)
        Tween:Create(fill, TweenInfo.new(0.08), { Size = UDim2.fromScale(pct, 1) }):Play()
        callback(rounded)
    end

    local holding = false
    labelFrame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then
            holding = true
            setFromX(UserInput:GetMouseLocation().X)
        end
    end)
    UserInput.InputEnded:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseButton1 then holding = false end
    end)
    UserInput.InputChanged:Connect(function(i)
        if holding and i.UserInputType == Enum.UserInputType.MouseMovement then
            setFromX(i.Position.X)
        end
    end)

    local sep = Instance.new("Frame")
    sep.BackgroundColor3 = Color3.fromRGB(50, 52, 60)
    sep.BackgroundTransparency = 0.4
    sep.BorderSizePixel = 0
    sep.AnchorPoint = Vector2.new(0, 1)
    sep.Position = UDim2.new(0, 8, 1, 0)
    sep.Size = UDim2.new(1, -16, 0, 1)
    sep.ZIndex = labelFrame.ZIndex + 2
    sep.Parent = labelFrame

    return {
        Root = labelFrame,
        GetValue = function() return state.value end,
        SetValue = function(v)
            state.value = v
            val.Text = tostring(v)
            local pct = (v - min) / math.max(max - min, 1)
            fill.Size = UDim2.fromScale(pct, 1)
            callback(v)
        end,
    }
end

-- ============================================================
-- PATCH SECTION
-- ============================================================
local function patchSection(section)
    if not section or section.__t_patched then return section end
    section.__t_patched = true
    function section:AddCheckRow(cfg)
        local lbl = self:AddLabel(" ")
        if not lbl or not lbl.Root then return nil end
        return injectRow(lbl.Root, cfg)
    end
    function section:AddKeybindRow(cfg)
        if cfg.Keybind == nil then cfg.Keybind = "None" end
        return self:AddCheckRow(cfg)
    end
    function section:AddThinSlider(cfg)
        local lbl = self:AddLabel(" ")
        if not lbl or not lbl.Root then return nil end
        return injectSlider(lbl.Root, cfg)
    end
    return section
end

-- Helper pra traduzir labels de seções criadas pela NL
local function registerSectionLabel(section, key)
    if not section or not section.Root then return end
    pcall(function()
        for _, child in ipairs(section.Root:GetChildren()) do
            if child:IsA("TextLabel") then
                registerTranslatable(child, key)
                child.Text = t(key)
                break
            end
        end
    end)
end

-- ============================================================
-- JANELA
-- ============================================================
local win = NL:CreateWindow({
    Name = "tetescr hub",
    Content = "ar2",
    Size = UDim2.fromOffset(1000, 540),
    ConfigFolder = CONFIG_FOLDER,
    Enable3DRenderer = false,
    Keybind = "P",
    Theme = "Dark",
    Accent = ACCENT_DEFAULT,
    Logo = "",
})

-- ============================================================
-- ORDEM DAS ABAS
-- ============================================================
local TAB_ORDER = {
    { name = "ESP",       key = "tab_esp"      },
    { name = "Combat",    key = "tab_combat"   },
    { name = "Movement",  key = "tab_movement" },
    { name = "Cars",      key = "tab_cars"     },
    { name = "World",     key = "tab_world"    },
    { name = "Radar",     key = "tab_radar"    },
    { name = "Teleport",  key = "tab_teleport" },
    { name = "Diversos",  key = "tab_diversos" },
    { name = "Cosmetics", key = "tab_cos"      },
    { name = "Config",    key = "tab_config"   },
}

local TABS = {}
for _, entry in ipairs(TAB_ORDER) do
    TABS[entry.name] = win:AddTab({ Icon = "", Name = t(entry.key) })
end

-- ============================================================
-- CORES POR ABA
-- ============================================================
local CAT = {
    ["ESP"]       = Color3.fromRGB(90, 220, 120),
    ["Combat"]    = Color3.fromRGB(255, 90, 90),
    ["Movement"]  = Color3.fromRGB(90, 180, 255),
    ["Cars"]      = Color3.fromRGB(255, 200, 60),
    ["World"]     = Color3.fromRGB(120, 200, 255),
    ["Radar"]     = Color3.fromRGB(255, 150, 60),
    ["Teleport"]  = Color3.fromRGB(180, 130, 255),
    ["Diversos"]  = Color3.fromRGB(200, 200, 210),
    ["Cosmetics"] = Color3.fromRGB(255, 130, 220),
    ["Config"]    = Color3.fromRGB(180, 180, 190),
}

local function R(section, name, key, default, keybind, cat)
    return section:AddCheckRow({
        Name = name, TransKey = key, Default = default, Keybind = keybind,
        Accent = CAT[cat],
    })
end

local function S(section, name, key, min, max, default, cat)
    return section:AddThinSlider({
        Name = name, TransKey = key, Min = min, Max = max,
        Default = default, Rounding = 0, Accent = CAT[cat],
    })
end

-- ============================================================
-- CONTEÚDO
-- ============================================================
-- ESP
local espL_section = TABS["ESP"]:AddSection({ Name = t("sec_player_visuals") })
registerSectionLabel(espL_section, "sec_player_visuals")
local espL = patchSection(espL_section)
espL:AddKeybindRow({ Name = "Enable ESP", TransKey = "opt_enable_esp", Default = false, Keybind = "T", Accent = CAT.ESP })
R(espL, "Player Name",     "opt_player_name",     true,  nil, "ESP")
R(espL, "Player Box",      "opt_player_box",      true,  nil, "ESP")
R(espL, "Player Health",   "opt_player_health",   true,  nil, "ESP")
R(espL, "Player Distance", "opt_player_distance", true,  nil, "ESP")
R(espL, "Player Skeleton", "opt_player_skeleton", false, nil, "ESP")
R(espL, "Player Chams",    "opt_player_chams",    false, nil, "ESP")

local espR_section = TABS["ESP"]:AddSection({ Name = t("sec_filters"), Position = "right" })
registerSectionLabel(espR_section, "sec_filters")
local espR = patchSection(espR_section)
R(espR, "Ignore Teammates", "opt_ignore_teammates", false, nil, "ESP")
R(espR, "Visible Only",     "opt_visible_only",     false, nil, "ESP")
S(espR, "Render Distance", "opt_render_distance", 100, 5000, 2000, "ESP")

-- Combat
local cbL_section = TABS["Combat"]:AddSection({ Name = t("sec_aim") })
registerSectionLabel(cbL_section, "sec_aim")
local cbL = patchSection(cbL_section)
cbL:AddKeybindRow({ Name = "Silent Aim", TransKey = "opt_silent_aim", Default = false, Keybind = "None", Accent = CAT.Combat })
cbL:AddKeybindRow({ Name = "Rage Bot",   TransKey = "opt_rage_bot",   Default = false, Keybind = "L",    Accent = CAT.Combat })
R(cbL, "Wall Check", "opt_wall_check", true, nil, "Combat")
S(cbL, "FOV",       "opt_fov",       20, 500, 180, "Combat")
S(cbL, "Smoothing", "opt_smoothing", 1,  60,  18,  "Combat")

local cbR_section = TABS["Combat"]:AddSection({ Name = t("sec_weapon"), Position = "right" })
registerSectionLabel(cbR_section, "sec_weapon")
local cbR = patchSection(cbR_section)
R(cbR, "No Recoil", "opt_no_recoil", false, nil, "Combat")
R(cbR, "No Spread", "opt_no_spread", false, nil, "Combat")
R(cbR, "Wallbang",  "opt_wallbang",  false, nil, "Combat")
R(cbR, "Hitmarker", "opt_hitmarker", false, nil, "Combat")

-- Movement
local mvL_section = TABS["Movement"]:AddSection({ Name = t("sec_movement_player") })
registerSectionLabel(mvL_section, "sec_movement_player")
local mvL = patchSection(mvL_section)
mvL:AddKeybindRow({ Name = "Speed Hack", TransKey = "opt_speed_hack", Default = false, Keybind = "None", Accent = CAT.Movement })
S(mvL, "Speed %", "opt_speed_pct", 100, 400, 140, "Movement")
R(mvL, "Infinite Jump", "opt_infinite_jump", false, nil, "Movement")
R(mvL, "Noclip",        "opt_noclip",        false, nil, "Movement")
R(mvL, "Bunny Hop",     "opt_bunny_hop",     false, nil, "Movement")

local mvR_section = TABS["Movement"]:AddSection({ Name = t("sec_fly"), Position = "right" })
registerSectionLabel(mvR_section, "sec_fly")
local mvR = patchSection(mvR_section)
mvR:AddKeybindRow({ Name = "Fly", TransKey = "opt_fly", Default = false, Keybind = "F", Accent = CAT.Movement })
S(mvR, "Fly Speed", "opt_fly_speed", 10, 1000, 300, "Movement")
R(mvR, "Car Fly", "opt_car_fly", false, nil, "Movement")

-- Cars
local crL_section = TABS["Cars"]:AddSection({ Name = t("sec_cars_tuning") })
registerSectionLabel(crL_section, "sec_cars_tuning")
local crL = patchSection(crL_section)
R(crL, "Infinite Fuel", "opt_inf_fuel", false, nil, "Cars")
R(crL, "Boost",         "opt_boost",    false, nil, "Cars")
R(crL, "Godmode",       "opt_godmode",  false, nil, "Cars")

local crR_section = TABS["Cars"]:AddSection({ Name = t("sec_cars_noclip"), Position = "right" })
registerSectionLabel(crR_section, "sec_cars_noclip")
local crR = patchSection(crR_section)
R(crR, "Vehicle Noclip", "opt_vehicle_noclip", false, nil, "Cars")
S(crR, "Noclip Speed", "opt_noclip_speed", 10, 1000, 90, "Cars")

-- World
local wdL_section = TABS["World"]:AddSection({ Name = t("sec_lighting") })
registerSectionLabel(wdL_section, "sec_lighting")
local wdL = patchSection(wdL_section)
wdL:AddKeybindRow({ Name = "Fullbright", TransKey = "opt_fullbright", Default = false, Keybind = "None", Accent = CAT.World })
R(wdL, "No Fog", "opt_no_fog", false, nil, "World")
S(wdL, "Brightness", "opt_brightness", 0, 10, 3, "World")
S(wdL, "Time",       "opt_time",       0, 24, 14, "World")

-- Radar
local rdL_section = TABS["Radar"]:AddSection({ Name = t("sec_radar_main") })
registerSectionLabel(rdL_section, "sec_radar_main")
local rdL = patchSection(rdL_section)
R(rdL, "Enabled",  "opt_radar_enabled", false, nil, "Radar")
R(rdL, "Names",    "opt_radar_names",   true,  nil, "Radar")
R(rdL, "Distance", "opt_radar_dist",    true,  nil, "Radar")

local rdR_section = TABS["Radar"]:AddSection({ Name = t("sec_minimap"), Position = "right" })
registerSectionLabel(rdR_section, "sec_minimap")
local rdR = patchSection(rdR_section)
R(rdR, "Enabled", "opt_radar_enabled", false, nil, "Radar")
S(rdR, "Size", "opt_minimap_size", 100, 600, 200, "Radar")

-- Teleport
local tpL_section = TABS["Teleport"]:AddSection({ Name = t("sec_tp_player") })
registerSectionLabel(tpL_section, "sec_tp_player")
local tpL = patchSection(tpL_section)
tpL:AddLabel(t("opt_target")):AddDropdown({
    Values = { "None", "Player1", "Player2" }, Default = "None", Callback = function() end,
})
S(tpL, "Behind", "opt_behind", -30, 30, 10, "Teleport")
tpL:AddButton({ Name = t("opt_tp_to_target"), Callback = function() end })
tpL:AddButton({ Name = t("opt_open_map"),     Callback = function() end })

-- Diversos
local dvL_section = TABS["Diversos"]:AddSection({ Name = t("sec_freeze") })
registerSectionLabel(dvL_section, "sec_freeze")
local dvL = patchSection(dvL_section)
R(dvL, "Freeze Zombies", "opt_freeze_zombies", false, nil, "Diversos")
R(dvL, "Whole Server",   "opt_whole_server",   false, nil, "Diversos")
S(dvL, "Radius", "opt_radius", 50, 2000, 300, "Diversos")

local dvR_section = TABS["Diversos"]:AddSection({ Name = t("sec_camera"), Position = "right" })
registerSectionLabel(dvR_section, "sec_camera")
local dvR = patchSection(dvR_section)
R(dvR, "FOV Zoom",    "opt_fov_zoom",    false, nil, "Diversos")
R(dvR, "Anti-Aim",    "opt_anti_aim",    false, nil, "Diversos")
R(dvR, "Jump Circle", "opt_jump_circle", false, nil, "Diversos")

-- Cosmetics
local csL_section = TABS["Cosmetics"]:AddSection({ Name = t("sec_unlocker") })
registerSectionLabel(csL_section, "sec_unlocker")
local csL = patchSection(csL_section)
R(csL, "Enable Selection", "opt_enable_selection", false, nil, "Cosmetics")
csL:AddButton({ Name = t("opt_remove_all_cos"), Callback = function() end })

-- Config — Language (topo)
local langSection_section = TABS["Config"]:AddSection({ Name = t("sec_language") })
registerSectionLabel(langSection_section, "sec_language")
local langSection = patchSection(langSection_section)
local langList = {}
for k, _ in pairs(LOCALES) do table.insert(langList, k) end
table.sort(langList)

langSection:AddLabel("Language"):AddDropdown({
    Values = langList,
    Default = CURRENT_LANG,
    Callback = function(chosen)
        if not LOCALES[chosen] then return end
        applyLocale(chosen)
        saveLocale(chosen)
    end,
})

-- Config — Keybinds
local cfL_section = TABS["Config"]:AddSection({ Name = t("sec_keybinds") })
registerSectionLabel(cfL_section, "sec_keybinds")
local cfL = patchSection(cfL_section)
cfL:AddLabel(t("opt_click_row"))
cfL:AddKeybindRow({ Name = "Player ESP", TransKey = "opt_player_esp_kb", Default = false, Keybind = "None", Accent = CAT.Config })
cfL:AddKeybindRow({ Name = "Silent Aim", TransKey = "opt_silent_aim_kb", Default = false, Keybind = "None", Accent = CAT.Config })
cfL:AddKeybindRow({ Name = "Fly",        TransKey = "opt_fly_kb",        Default = false, Keybind = "None", Accent = CAT.Config })
cfL:AddButton({ Name = t("opt_reset_kb"), Callback = function() end })

local cfR_section = TABS["Config"]:AddSection({ Name = t("sec_window"), Position = "right" })
registerSectionLabel(cfR_section, "sec_window")
local cfR = patchSection(cfR_section)
cfR:AddButton({ Name = t("opt_save_config"),    Callback = function() end })
cfR:AddButton({ Name = t("opt_load_config"),    Callback = function() end })
cfR:AddButton({ Name = t("opt_reset_defaults"), Callback = function() end })

-- Aplica o idioma salvo logo após montar
applyLocale(CURRENT_LANG)

-- ============================================================
-- ESPERA A NL MONTAR
-- ============================================================
task.wait(0.6)

-- ============================================================
-- LOCALIZA WINDOWFRAME
-- ============================================================
local function findWindowFrame()
    local g = NL.ScreenGui
    if not g then return nil end
    for _, obj in ipairs(g:GetDescendants()) do
        if obj:IsA("Frame") and obj.Size.X.Offset == 1000 and obj.Size.Y.Offset == 540 then
            return obj
        end
    end
    return nil
end

local wf = findWindowFrame()
if not wf then
    warn("[v9.7] windowFrame NÃO ENCONTRADO")
    if NL.ScreenGui then NL.ScreenGui.Enabled = true end
    return
end

local leftMenu, rightMenu
for _, child in ipairs(wf:GetChildren()) do
    if child:IsA("Frame") then
        if child.Size.X.Offset == 165 then leftMenu = child end
        if child.Position.X.Offset == 166 and child.Size.X.Offset < 0 then rightMenu = child end
    end
end

if leftMenu then
    leftMenu.ClipsDescendants = true
    for _, child in ipairs(leftMenu:GetChildren()) do
        if child:IsA("Frame") then
            local isHeadFrame = false
            for _, sub in ipairs(child:GetChildren()) do
                if sub:IsA("TextLabel") and sub.Text == "tetescr hub" then
                    isHeadFrame = true break
                end
            end
            if isHeadFrame then
                child.Position = UDim2.new(0, 0, 0, 0)
                child.Size = UDim2.new(0, 220, 0, 38)
                child.Visible = true
            else
                child.Visible = false
            end
        elseif child:IsA("ScrollingFrame") then
            child.Visible = false
            for _, sub in ipairs(child:GetDescendants()) do
                pcall(function() sub.Visible = false end)
            end
        end
    end
    leftMenu.BackgroundTransparency = 1
end

if rightMenu then
    rightMenu.Position = UDim2.new(0, 0, 0, 38)
    rightMenu.Size = UDim2.new(1, 0, 1, -38)
    rightMenu.ClipsDescendants = true
    for _, child in ipairs(rightMenu:GetChildren()) do
        if child:IsA("Frame") and child.Size.Y.Offset == 50 and child.Position.Y.Offset == 0 then
            child.Visible = false
        end
    end
    for _, child in ipairs(rightMenu:GetChildren()) do
        if child:IsA("Frame") and child.Position.Y.Offset == 50 then
            child.Position = UDim2.new(0, 0, 0, 0)
            child.Size = UDim2.new(1, 0, 1, 0)
        end
    end
end

-- ============================================================
-- BARRA DE CHIPS
-- ============================================================
local tabBar = Instance.new("Frame")
tabBar.Name = "tetescr_tabbar"
tabBar.BackgroundColor3 = Color3.fromRGB(20, 20, 24)
tabBar.BackgroundTransparency = 0.1
tabBar.BorderSizePixel = 0
tabBar.Position = UDim2.new(0, 220, 0, 0)
tabBar.Size = UDim2.new(1, -220, 0, 38)
tabBar.ZIndex = 100
tabBar.Parent = wf

local layout = Instance.new("UIListLayout")
layout.FillDirection = Enum.FillDirection.Horizontal
layout.HorizontalAlignment = Enum.HorizontalAlignment.Left
layout.VerticalAlignment = Enum.VerticalAlignment.Center
layout.Padding = UDim.new(0, 6)
layout.SortOrder = Enum.SortOrder.LayoutOrder
layout.Parent = tabBar

local pad = Instance.new("UIPadding")
pad.PaddingLeft = UDim.new(0, 10)
pad.PaddingRight = UDim.new(0, 10)
pad.Parent = tabBar

local tabButtons = {}

local function setActive(idx)
    for i, btn in ipairs(tabButtons) do
        local isActive = (i == idx)
        local color = btn:GetAttribute("accent") or ACCENT_DEFAULT
        btn:SetAttribute("active", isActive)
        Tween:Create(btn, TweenInfo.new(0.15), {
            BackgroundColor3 = isActive and Color3.fromRGB(50, 55, 65) or Color3.fromRGB(35, 38, 44),
            TextColor3 = isActive and color or Color3.fromRGB(200, 200, 210),
        }):Play()
    end
end

local chipRefs = {}

for i, entry in ipairs(TAB_ORDER) do
    local tab = TABS[entry.name]
    local color = CAT[entry.name] or ACCENT_DEFAULT

    local btn = Instance.new("TextButton")
    btn.Name = "tab_" .. entry.name
    btn.BackgroundColor3 = Color3.fromRGB(35, 38, 44)
    btn.BorderSizePixel = 0
    btn.AutoButtonColor = false
    btn.Font = Enum.Font.GothamBold
    btn.Text = t(entry.key)
    btn.TextColor3 = Color3.fromRGB(200, 200, 210)
    btn.TextSize = 12
    btn.ZIndex = 101

    local ts = TextSvc:GetTextSize(t(entry.key), 12, Enum.Font.GothamBold, Vector2.new(1000, 1000))
    btn.Size = UDim2.fromOffset(ts.X + 24, 26)

    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(0, 3)
    c.Parent = btn

    local s = Instance.new("UIStroke")
    s.Color = color
    s.Thickness = 1
    s.Transparency = 0.7
    s.Parent = btn

    btn:SetAttribute("accent", color)
    btn.LayoutOrder = i
    btn.Parent = tabBar

    btn.MouseEnter:Connect(function()
        if btn:GetAttribute("active") then return end
        Tween:Create(btn, TweenInfo.new(0.1), {
            BackgroundColor3 = Color3.fromRGB(50, 55, 65),
        }):Play()
    end)
    btn.MouseLeave:Connect(function()
        if btn:GetAttribute("active") then return end
        Tween:Create(btn, TweenInfo.new(0.1), {
            BackgroundColor3 = Color3.fromRGB(35, 38, 44),
        }):Play()
    end)

    btn.MouseButton1Click:Connect(function()
        for _, t2 in ipairs(win.Tabs) do
            pcall(function() t2.SetValue(false) end)
        end
        pcall(function() tab.SetValue(true) end)
        setActive(i)
        pcall(function() NL:SetAccentColor(color) end)
    end)

    tabButtons[i] = btn

    -- Registra o chip pra tradução + guarda referência pra redimensionar depois
    table.insert(chipRefs, { btn = btn, key = entry.key })
    registerTranslatable(btn, entry.key)
end

setActive(1)

-- Recalcula larguras dos chips ao trocar idioma
local origApplyLocale = applyLocale
applyLocale = function(lang)
    origApplyLocale(lang)
    task.wait(0.05)
    for _, ref in ipairs(chipRefs) do
        pcall(function()
            local newText = t(ref.key)
            local size = TextSvc:GetTextSize(newText, 12, Enum.Font.GothamBold, Vector2.new(1000, 1000))
            ref.btn.Size = UDim2.fromOffset(size.X + 24, 26)
        end)
    end
end

-- ============================================================
-- PATCH RETANGULAR
-- ============================================================
local function applySquarePatch()
    local g = NL.ScreenGui
    if not g then return end
    for _, obj in ipairs(g:GetDescendants()) do
        if obj:IsA("UICorner") then
            pcall(function() obj.CornerRadius = UDim.new(0, 0) end)
        end
    end
end

applySquarePatch()
task.spawn(function()
    while true do
        task.wait(0.5)
        pcall(applySquarePatch)
    end
end)

-- ============================================================
-- REABILITA O SCREENGUI
-- ============================================================
task.wait(0.15)
if NL.ScreenGui then
    NL.ScreenGui.Enabled = true
end

print("[v9.7] FIM — idioma:", CURRENT_LANG)
