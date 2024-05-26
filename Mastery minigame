import os
import random
import requests

def get_latest_champion_data():
    versions_url = 'https://ddragon.leagueoflegends.com/api/versions.json'
    response = requests.get(versions_url)
    if response.status_code == 200:
        versions = response.json()
        latest_version = versions[0]
        
        champions_url = f'https://ddragon.leagueoflegends.com/cdn/{latest_version}/data/en_US/champion.json'
        response = requests.get(champions_url)
        if response.status_code == 200:
            champions_data = response.json()['data']
            return champions_data
        else:
            print(f"Erro ao acessar dados dos campeões: {response.status_code}")
            print("Mensagem de erro:", response.json())
    else:
        print(f"Erro ao acessar versões: {response.status_code}")
        print("Mensagem de erro:", response.json())
    return None

def randomize_champion(champions_data):
    champion_names = list(champions_data.keys())
    random_champion = random.choice(champion_names)
    return random_champion

def get_champion_mastery_from_files(champion_name):
    sanitized_champion_name = champion_name.replace(' ', '_')
    files = [f for f in os.listdir('.') if f.startswith('maestrias_') and f.endswith('.txt')]
    mastery_info = {}

    for file in files:
        with open(file, 'r') as f:
            found = False
            for line in f:
                if line.startswith(sanitized_champion_name):
                    found = True
                    parts = line.split(': ')[1].split(' ')
                    champ_points = int(parts[0])
                    champ_level = int(parts[2].replace('(', '').replace(')', ''))
                    summoner_name = file.split('_')[1].replace('.txt', '')
                    mastery_info[summoner_name] = (champ_points, champ_level)
            if not found:
                mastery_info[file.split('_')[1].replace('.txt', '')] = (0, 0)
    return mastery_info

def main():
    champions_data = get_latest_champion_data()
    if champions_data:
        random_champion = randomize_champion(champions_data)
        mastery_info = get_champion_mastery_from_files(random_champion)
        
        if mastery_info:
            invocadores = list(mastery_info.items())
            random.shuffle(invocadores)
            attempts = min(6, len(invocadores))
            invocadores_mostrados = 1

            print("Os invocadores com maestria neste campeão:")
            for summoner, (points, level) in invocadores[:invocadores_mostrados]:
                print(f"Invocador: {summoner}, Maestria: {points}, Nível: {level}")

            while attempts > 0:
                guess = input(f"\nVocê tem {attempts} tentativas restantes. Qual é o nome do campeão randomizado? ")
                if guess.lower() == random_champion.lower():
                    print("\nParabéns! Você acertou o campeão!")
                    break
                else:
                    attempts -= 1
                    invocadores_mostrados += 1
                    if attempts > 0:
                        print(f"\nResposta incorreta. Você ainda tem {attempts} tentativas.")
                        print(f"Próximo invocador:")
                        for summoner, (points, level) in invocadores[:invocadores_mostrados]:
                            print(f"Invocador: {summoner}, Maestria: {points}, Nível: {level}")
                    else:
                        print(f"\nSuas tentativas acabaram. O campeão randomizado era '{random_champion}'.")
        else:
            print("Nenhuma informação de maestria encontrada para o campeão randomizado.")

if __name__ == "__main__":
    main()
