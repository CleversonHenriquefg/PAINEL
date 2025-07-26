#!/usr/bin/env python3

import requests
import random
import string
import socket
import os
import re
from datetime import datetime

def limpar():
    os.system('clear' if os.name == 'posix' else 'cls')

def banner():
    print("""
=========================================
        ETHICAL - CLI PAINEL
        By Cleverson Gomes
=========================================
""")

def menu():
    print("""
[1] Verificar status HTTP de um site
[2] Gerar senha segura
[3] Verificar portas abertas em host
[4] Ver meu IP público
[5] Analisar replay OTC (IQ Option)
[0] Sair
""")

def status_site():
    url = input("Digite a URL (ex.: https://www.google.com): ")
    try:
        r = requests.get(url, timeout=5)
        print(f"Status Code: {r.status_code}")
    except Exception as e:
        print(f"Erro ao acessar: {e}")

def gerar_senha():
    tamanho = int(input("Tamanho da senha: "))
    caracteres = string.ascii_letters + string.digits + string.punctuation
    senha = ''.join(random.choice(caracteres) for _ in range(tamanho))
    print(f"Senha gerada: {senha}")

def scan_portas():
    host = input("Digite o host (ex.: scanme.nmap.org): ")
    portas = [21, 22, 23, 25, 53, 80, 443, 3306, 8080]
    print(f"Escaneando portas em {host}...")
    for porta in portas:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(1)
        result = sock.connect_ex((host, porta))
        if result == 0:
            print(f"[OPEN] Porta {porta}")
        else:
            print(f"[CLOSED] Porta {porta}")
        sock.close()

def meu_ip():
    try:
        ip = requests.get("https://api.ipify.org").text
        print(f"Seu IP público é: {ip}")
    except:
        print("Erro ao buscar IP.")

def analisar_replay():
    caminho = input("Caminho do arquivo de replay: ")
    if not os.path.isfile(caminho):
        print("Arquivo não encontrado.")
        return
    with open(caminho, "r", encoding="utf-8", errors="ignore") as f:
        conteudo = f.read()
    match = re.search(r"(\d{4}-\d{2}-\d{2})", conteudo)
    if not match:
        print("Data n\u00e3o encontrada no arquivo.")
        return
    data_str = match.group(1)
    try:
        data = datetime.strptime(data_str, "%Y-%m-%d")
        dia_semana = data.strftime("%A")
        print(f"Data encontrada: {data_str} - {dia_semana}")
    except ValueError:
        print(f"Formato de data inesperado: {data_str}")

def main():
    while True:
        limpar()
        banner()
        menu()
        op = input("Escolha: ")
        if op == '1':
            status_site()
        elif op == '2':
            gerar_senha()
        elif op == '3':
            scan_portas()
        elif op == '4':
            meu_ip()
        elif op == '5':
            analisar_replay()
        elif op == '0':
            print("Saindo...")
            break
        else:
            print("Opção inválida.")
        input("\nPressione Enter para continuar...")

if __name__ == "__main__":
    main()
