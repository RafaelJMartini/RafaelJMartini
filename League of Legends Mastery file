import requests

# Substitua pelo seu Riot API key
api_key = 'CHANGE FOR YOUR RIOT API KEY'
game_name = 'YOUR NICKNAME'
tag_line = 'YOUR TAG'
account_region = 'YOUR REGION'  # Região apropriada para a endpoint da conta Riot (mine is 'americas')
game_region = 'YOUR SERVER REGION'  # Região apropriada para a endpoint do jogo (mine is 'br1')

# Codificar gameName e tagLine para a URL
game_name_encoded = requests.utils.quote(game_name)
tag_line_encoded = requests.utils.quote(tag_line)

# URL para obter o PUUID do invocador
account_url = f'https://{account_region}.api.riotgames.com/riot/account/v1/accounts/by-riot-id/{game_name_encoded}/{tag_line_encoded}?api_key={api_key}'

response = requests.get(account_url)
if response.status_code == 200:
    account_data = response.json()
    summoner_puuid = account_data['puuid']
    
    # URL para obter os dados de maestria usando PUUID
    mastery_url = f'https://{game_region}.api.riotgames.com/lol/champion-mastery/v4/champion-masteries/by-puuid/{summoner_puuid}?api_key={api_key}'
    
    response = requests.get(mastery_url)
    if response.status_code == 200:
        mastery_data = response.json()
        
        # URL para obter a versão mais recente dos dados dos campeões
        versions_url = 'https://ddragon.leagueoflegends.com/api/versions.json'
        response = requests.get(versions_url)
        if response.status_code == 200:
            versions = response.json()
            latest_version = versions[0]
            
            # URL para obter dados estáticos dos campeões com a versão mais recente
            champions_url = f'https://ddragon.leagueoflegends.com/cdn/{latest_version}/data/en_US/champion.json'
            
            response = requests.get(champions_url)
            if response.status_code == 200:
                champions_data = response.json()['data']
                champion_id_to_name = {int(champions_data[champion]['key']): champion for champion in champions_data}
                
                # Nome do arquivo com game_name
                sanitized_game_name = game_name.replace(' ', '_')  # Substituir espaços por underscores
                file_name = f'maestrias_{sanitized_game_name}.txt'
                
                with open(file_name, 'w') as file:
                    for champion in mastery_data:
                        champ_id = champion['championId']
                        champ_points = champion['championPoints']
                        champ_level = champion['championLevel']
                        champ_name = champion_id_to_name.get(champ_id, 'Unknown Champion')
                        
                        file.write(f'{champ_name}: {champ_points} (Level {champ_level})\n')
                        
                print(f"Dados de maestria salvos em '{file_name}'.")
            else:
                print(f"Erro ao acessar dados dos campeões: {response.status_code}")
                print("Mensagem de erro:", response.json())
        else:
            print(f"Erro ao acessar versões: {response.status_code}")
            print("Mensagem de erro:", response.json())
    else:
        print(f"Erro ao acessar dados de maestria: {response.status_code}")
        print("Mensagem de erro:", response.json())
else:
    print(f"Erro ao acessar dados da conta: {response.status_code}")
    print("Mensagem de erro:", response.json())
