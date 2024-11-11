def run_simulation():
    weather = WeatherSystem()
    grid = Grid(weather)
    storage = EnergyStorage(50)

    while True:
        print("\n--- МЕНЮ ---")
        print("1. Додати електростанцію")
        print("2. Додати споживача")
        print("3. Переглянути електростанції")
        print("4. Переглянути споживачів")
        print("5. Змінити погоду")
        print("6. Переглянути погодні дані")
        print("7. Розподіл енергії")
        print("8. Балансування навантаження")
        print("9. Аварійна ситуація")
        print("10. Вийти")

        choice = input("Виберіть опцію: ")

        if choice == '1':
            add_plant_to_grid(grid, weather)
        elif choice == '2':
            add_consumer_to_grid(grid)
        elif choice == '3':
            grid.display_power_plants()
        elif choice == '4':
            grid.display_consumers()
        elif choice == '5':
            new_weather = input("Введіть новий стан погоди (clear, cloudy, windy, stormy): ").strip().lower()
            weather.change_weather(new_weather)
            grid.update_weather_effects()
        elif choice == '6':
            weather.display_weather()
        elif choice == '7':
            grid.distribute_energy()
        elif choice == '8':
            grid.balance_load()
        elif choice == '9':
            grid.handle_emergency()
        elif choice == '10':
            break
        else:
            print("Неправильний вибір. Спробуйте знову.")

        # Зберігання надлишкової енергії або випуск у разі дефіциту
        total_generation = sum([plant.generate_power() for plant in grid.power_plants])
        total_demand = sum([consumer.demand_energy() for consumer in grid.consumers])
        excess_energy = total_generation - total_demand
        if excess_energy > 0:
            storage.store_energy(excess_energy)
        else:
            storage.release_energy(abs(excess_energy))


run_simulation()
