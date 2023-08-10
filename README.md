# Loto
#Análise
import pandas as pd

# Ler o arquivo Excel com os resultados dos concursos
data = pd.read_excel("D:\Programa\Sorte_loto.xlsx")  # Substitua pelo caminho correto

# Definir o número máximo de repetições retroativas a serem analisadas
num_repeticoes_analisadas = 5

# Função para realizar a análise de repetições retroativas para um concurso
def analisar_repeticoes(concurso_base):
    base_concurso = set(data.loc[data['Concurso'] == concurso_base, 'bola 1':'bola 15'].values[0])
    primeiras = []
    segundas = []
    terceiras = []
    quartas = []
    quintas = []

    for i in range(1, num_repeticoes_analisadas + 1):
        concurso_atual = concurso_base - i
        
        # Verificar se o concurso_atual existe no DataFrame
        if concurso_atual in data['Concurso'].values:
            dezenas_concurso_atual = set(data.loc[data['Concurso'] == concurso_atual, 'bola 1':'bola 15'].values[0])
            dezenas_repetidas = base_concurso.intersection(dezenas_concurso_atual)

            if len(dezenas_repetidas) == 0:
                break

            if i == 1:
                primeiras = list(dezenas_repetidas)
            elif i == 2:
                segundas = list(dezenas_repetidas)
            elif i == 3:
                terceiras = list(dezenas_repetidas)
            elif i == 4:
                quartas = list(dezenas_repetidas)
            elif i == 5:
                quintas = list(dezenas_repetidas)

            base_concurso = dezenas_repetidas
        else:
            break

    return primeiras, segundas, terceiras, quartas, quintas

# Organizar e imprimir as dezenas repetidas por categoria para cada concurso
def organizar_e_imprimir_dezenas_repetidas():
    concursos_dict_list = []
    
    for concurso_base in data['Concurso']:
        primeiras, segundas, terceiras, quartas, quintas = analisar_repeticoes(concurso_base)
        
        # Criar um novo dicionário para armazenar as informações do concurso atual
        concurso_dict = {}
        concurso_dict['concurso'] = concurso_base
        
        if primeiras:
            concurso_dict['primeiras'] = {'dezenas': primeiras, 'total': len(primeiras)}
        
        if segundas:
            concurso_dict['segundas'] = {'dezenas': segundas, 'total': len(segundas)}
        
        if terceiras:
            concurso_dict['terceiras'] = {'dezenas': terceiras, 'total': len(terceiras)}
        
        if quartas:
            concurso_dict['quartas'] = {'dezenas': quartas, 'total': len(quartas)}
        
        if quintas:
            concurso_dict['quintas'] = {'dezenas': quintas, 'total': len(quintas)}
        
        # Adicionar o dicionário do concurso atual à lista de dicionários
        concursos_dict_list.append(concurso_dict)
    
    # Imprimir os resultados organizados por categoria para cada concurso
    for concursos_dict in concursos_dict_list:
        print(f"Concurso {concursos_dict['concurso']}:")
        
        if 'primeiras' in concursos_dict.keys():
            print(f"Dezenas repetidas 'primeiras': {concursos_dict['primeiras']['dezena']}   Total {concursos_dict['primeiras']['total']}")
        
        if 'segundas' in concursos_dict.keys():
            print(f"Dezenas repetidas 'segundas': {concursos_dict['segundas']['dezena']}   Total {concursos_dict['segundas']['total']}")
            
        if 'terceiras' in concursos_dict.keys():
            print(f"Dezenas repetidas 'terceiras': {concursos_dict['terceiras']['dezena']}   Total {concursos_dict['terceiras']['total']}")
        
        if 'quartas' in concursos_dict.keys():
            print(f"Dezenas repetidas 'quartas': {concursos_dict['quartas']['dezena']}   Total {concursos_dict['quartas']['total']}")
            
        if 'quintas' in concursos_dict.keys():
            print(f"Dezenas repetidas 'quintas': {concursos_dict['quintas']['dezena']}   Total {concursos_dict['quintas']['total']}")
