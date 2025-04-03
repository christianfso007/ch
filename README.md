class Veiculo:
    def __init__(self, placa, tipo, capacidade):
        self.placa = placa
        self.tipo = tipo  # Ex: Caminhão, Van, Moto
        self.capacidade = capacidade  # Capacidade de carga (em kg)

    def __str__(self):
        return f"Veículo {self.tipo} - Placa: {self.placa}, Capacidade: {self.capacidade}kg"


class Cliente:
    def __init__(self, nome, endereco):
        self.nome = nome
        self.endereco = endereco

    def __str__(self):
        return f"Cliente: {self.nome}, Endereço: {self.endereco}"


class Entrega:
    def __init__(self, cliente, veiculo, peso, destino, distancia):
        self.cliente = cliente
        self.veiculo = veiculo
        self.peso = peso
        self.destino = destino
        self.distancia = distancia
        self.status = "Pendente"  # Status pode ser "Pendente", "Em trânsito" ou "Entregue"
        self.valor_frete = 0  # Valor do frete

    def calcular_frete(self):
        preco_por_kg_km = 0.05  # Exemplo de valor por kg por km (R$ 0,05)
        self.valor_frete = self.peso * self.distancia * preco_por_kg_km
        return self.valor_frete

    def iniciar_entrega(self):
        if self.peso <= self.veiculo.capacidade:
            self.status = "Em trânsito"
            print(f"Entrega a caminho meu parceiro {self.cliente.nome}.")
        else:
            print(f"Contrata os vingadores para fazer a entrega, muito!")

    def concluir_entrega(self):
        if self.status == "Em trânsito":
            self.status = "Entregue"
            print(f"Entrega concluída para o cliente {self.cliente.nome}.")
        else:
            print(f"Erro: Não é possível concluir a entrega se ela não estiver em trânsito.")

    def __str__(self):
        return (f"Entrega para {self.cliente.nome} ({self.cliente.endereco}), "
                f"Peso: {self.peso}kg, Destino: {self.destino}, "
                f"Distância: {self.distancia}km, Status: {self.status}, "
                f"Valor do Frete: R${self.valor_frete:.2f}")


class Transportadora:
    def __init__(self):
        self.veiculos = []
        self.clientes = []
        self.entregas = []

    def adicionar_veiculo(self, veiculo):
        self.veiculos.append(veiculo)

    def adicionar_cliente(self, cliente):
        self.clientes.append(cliente)

    def criar_entrega(self):
        # Perguntar informações para criar a entrega
        print("\nPor favor, insira as informações para criar uma entrega.")

        # Perguntar o nome do cliente e o endereço
        nome_cliente = input("Informe o nome do cliente: ")
        endereco_cliente = input("Informe o endereço do cliente: ")
        cliente = Cliente(nome_cliente, endereco_cliente)

        # Adicionando o cliente à lista
        self.adicionar_cliente(cliente)

        # Perguntar o veículo
        print("\nVeículos disponíveis:")
        for idx, veiculo in enumerate(self.veiculos, start=1):
            print(f"{idx}. {veiculo}")
        veiculo_idx = int(input("\nEscolha o número do veículo (1, 2, ...): ")) - 1
        veiculo = self.veiculos[veiculo_idx]

        # Perguntar o peso da carga e o destino
        peso = float(input("\nInforme o peso da carga (em kg): "))
        destino = input("Informe o destino da entrega: ")

        # Perguntar a distância (em km)
        distancia = float(input("Informe a distância em km: "))

        # Criar a entrega
        entrega = Entrega(cliente, veiculo, peso, destino, distancia)
        self.entregas.append(entrega)

        # Calcular e mostrar o valor do frete
        valor_frete = entrega.calcular_frete()
        print(f"\nEntrega criada: {entrega}")
        print(f"Valor do frete calculado: R${valor_frete:.2f}")

    def listar_entregas(self):
        print("\nLista de entregas:")
        for entrega in self.entregas:
            print(entrega)


# Exemplo de uso do programa

# Criando veículos
veiculo1 = Veiculo("ABC-1234", "Caminhão", 10000)
veiculo2 = Veiculo("XYZ-5678", "Van", 2000)

# Criando a transportadora e registrando veículos
transportadora = Transportadora()
transportadora.adicionar_veiculo(veiculo1)
transportadora.adicionar_veiculo(veiculo2)

# Perguntar ao usuário para criar uma entrega
transportadora.criar_entrega()

# Listar entregas
transportadora.listar_entregas()

# Iniciar e concluir a entrega
entrega = transportadora.entregas[0]
entrega.iniciar_entrega()
entrega.concluir_entrega()

# Exibindo novamente as entregas
print("\nLista de entregas após atualização:")
transportadora.listar_entregas()

