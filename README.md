Estrutura base simulando as telas: login, cadastro, home, agenda, conta, carteira, clientes e chat em Python (exemplo com Flask)

from flask import Flask, render_template_string, request, redirect, url_for

app = Flask(name)

login_page = '''

<h2>Login</h2>
<form method="post">
  Email: <input type="email" name="email"><br>
  Senha: <input type="password" name="password"><br>
  <input type="submit" value="Entrar">
</form>
<a href="/register">Criar conta</a>
'''register_page = '''

<h2>Cadastro</h2>
<form method="post">
  Nome: <input type="text" name="name"><br>
  Profissão: <input type="text" name="profession"><br>
  Email: <input type="email" name="email"><br>
  Senha: <input type="password" name="password"><br>
  <input type="submit" value="Cadastrar">
</form>
<a href="/">Voltar ao login</a>
'''home_page = '''

<h2>Bem-vindo, {{ name }}</h2>
<form method="get">
  <input type="text" placeholder="Buscar serviço">
  <input type="text" placeholder="Localização">
</form>
<h3>Categorias em destaque</h3>
<ul><li>Médico</li><li>Advogado</li><li>Psicólogo</li></ul>
<h3>Recomendado para você</h3>
<ul><li>Ana - Psicóloga</li><li>Carlos - Dentista</li></ul>
<a href="/agenda">Agenda</a> | <a href="/conta">Conta</a> | <a href="/carteira">Carteira</a> | <a href="/clientes">Clientes</a>
'''agenda_page = '''

<h2>Agenda</h2>
<ul>
  <li>05/05/2025 - 10:00 - Ana (Consulta)</li>
  <li>06/05/2025 - 14:00 - Carlos (Avaliação)</li>
</ul>
<a href="/home?name=Usuário">Voltar</a>
'''conta_page = '''

<h2>Minha Conta</h2>
<p>Nome: João Silva</p>
<p>Profissão: Psicólogo</p>
<p>Descrição: Especialista em terapia cognitivo-comportamental</p>
<p>Plano atual: R$15/mês (Premium)</p>
<a href="/home?name=João">Voltar</a>
'''carteira_page = '''

<h2>Carteira</h2>
<p>Saldo: R$ 250,00</p>
<h3>Extrato:</h3>
<ul>
  <li>+ R$ 100 - Consulta Ana - 03/05/2025</li>
  <li>+ R$ 150 - Consulta Carlos - 01/05/2025</li>
</ul>
<a href="/home?name=João">Voltar</a>
'''clientes_page = '''

<h2>Clientes</h2>
<ul>
  <li><a href="/chat?cliente=Ana">Ana - Em atendimento</a></li>
  <li><a href="/chat?cliente=Carlos">Carlos - Em espera</a></li>
</ul>
<a href="/home?name=João">Voltar</a>
'''chat_page = '''

<h2>Chat com {{ cliente }}</h2>
<div style="border:1px solid #ccc; padding:10px; margin-bottom:10px;">
  <p><b>{{ cliente }}:</b> Olá, gostaria de confirmar a consulta.</p>
  <p><b>Você:</b> Claro, está confirmada para amanhã às 10h.</p>
</div>
<form method="post">
  <input type="text" name="mensagem" placeholder="Digite uma mensagem">
  <input type="submit" value="Enviar">
</form>
<a href="/clientes">Voltar aos clientes</a>
'''@app.route('/', methods=['GET', 'POST']) def login(): if request.method == 'POST': return redirect(url_for('home', name="Usuário")) return render_template_string(login_page)

@app.route('/register', methods=['GET', 'POST']) def register(): if request.method == 'POST': return redirect(url_for('home', name=request.form['name'])) return render_template_string(register_page)

@app.route('/home') def home(): name = request.args.get('name', 'Usuário') return render_template_string(home_page, name=name)

@app.route('/agenda') def agenda(): return render_template_string(agenda_page)

@app.route('/conta') def conta(): return render_template_string(conta_page)

@app.route('/carteira') def carteira(): return render_template_string(carteira_page)

@app.route('/clientes') def clientes(): return render_template_string(clientes_page)

@app.route('/chat', methods=['GET', 'POST']) def chat(): cliente = request.args.get('cliente', 'Cliente') return render_template_string(chat_page, cliente=cliente)

if name == 'main': app.run(debug=True)

