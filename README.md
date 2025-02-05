# turbo-carnival
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////77
<html>
<head>
    <title>TFB</title>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <script src="script.js"></script>
    <style>
        .error {
            color: red;
        }
    </style>
</head>

<body> 
<table width="50%" align="center" cellspacing="default" cellpadding="2" border="2">
    <!-- ZAGLAVLJE -->
    <tr bgcolor="#DDDDDD" align="center">
        <td colspan="3">
            <font face="Arial, Tahoma, Verdana" size="3">
                <b>II kolokvij</b>
            </font>
        </td>
    </tr>
	
    <tr>
        <td align="center">
            <table>
                <tr align="center" valign="top">
                    <td width="20%">
                        <img src="add-user.jpg" width="100px" align="right">
                    </td>
                    
                    <td width="70%">
                        <table>
                            <form onsubmit="dodaj_korisnika(); return false;">
                                <tr>
                                    <td>Ime i prezime:</td>
                                    <td><input type="text" id="imePrezime" placeholder="Unesite ime i prezime">
                                    <span id="imePrezimeError" class="error"></span></td>
                                </tr>
                                <tr>
                                    <td>E-mail:</td>
                                    <td><input type="text" id="email" placeholder="Unesite e-mail adresu">
                                    <span id="emailError" class="error"></span></td>
                                </tr>
                                <tr>
                                    <td>Korisničko ime:</td>
                                    <td><input type="text" id="korIme" placeholder="Unesite korisničko ime">
                                    <span id="korImeError" class="error"></span></td>
                                </tr>
                                <tr>
                                    <td>Lozinka:</td>
                                    <td><input type="password" id="lozinka" placeholder="Unesite lozinku">
                                    <span id="lozinkaError" class="error"></span></td>
                                </tr>
                                <tr>
                                    <td>Potvrda lozinke:</td>
                                    <td><input type="password" id="potvrdaLozinke" placeholder="Unesite lozinku">
                                    <span id="potvrdaLozinkeError" class="error"></span></td>
                                </tr>
                                <tr>
                                    <td>Uslovi registracije:</td>
                                    <td><input type="checkbox" id="uvjetPrihvacen" checked="checked"></td>
                                </tr>
                                <tr>
                                    <td colspan="2" align="center">
                                        <input type="submit" value="DODAJ">
                                        <input type="reset" value="PONIŠTI">
                                    </td>
                                </tr>
                            </form>
                        </table>
                    </td>
                </tr>
            </table>
			
            <hr>
			
            <table id="korisniciTabela" align="center" border="2">
                <!-- This table will be dynamically generated -->
            </table>
        </td>
    </tr>
    
    <!-- PODNOŽJE -->
    <tr bgcolor="#DDDDDD" align="center">
        <td>
            <i>Alijagić Sajra, 1279,</i> &copy; <i>2025</i>
        </td>
    </tr>
</table>

<script>
// Global array for storing users
let korisnici = [];

// Function to add a new user
function dodaj_korisnika() {
    // Clear previous errors
    clearErrors();

    // Validate form
    if (!validacija_forme()) {
        return;
    }

    // Create a user object
    let korisnik = {
        imePrezime: document.getElementById('imePrezime').value,
        email: document.getElementById('email').value,
        korIme: document.getElementById('korIme').value,
        lozinka: document.getElementById('lozinka').value,
        uvjetPrihvacen: document.getElementById('uvjetPrihvacen').checked
    };

    // Add user to the array
    korisnici.push(korisnik);

    // Display all users
    prikaziKorisnike();
}

// Function to validate the entire form
function validacija_forme() {
    let imePrezime = document.getElementById('imePrezime').value;
    let email = document.getElementById('email').value;
    let korIme = document.getElementById('korIme').value;
    let lozinka = document.getElementById('lozinka').value;
    let potvrdaLozinke = document.getElementById('potvrdaLozinke').value;

    let isValid = true;

    if (!validacija_ime_prezime(imePrezime)) isValid = false;
    if (!validacija_email(email)) isValid = false;
    if (!validacija_kor_ime(korIme)) isValid = false;
    if (!validacija_lozinke(lozinka)) isValid = false;
    if (!validacija_potvrde_lozinke(lozinka, potvrdaLozinke)) isValid = false;

    return isValid;
}

// Function to validate name and surname
function validacija_ime_prezime(tekst) {
    if (tekst.trim() === '') {
        document.getElementById('imePrezimeError').innerText = 'Unesite ime i prezime';
        return false;
    }
    return true;
}

// Function to validate email
function validacija_email(tekst) {
    if (tekst.trim() === '') {
        document.getElementById('emailError').innerText = 'Unesite e-mail adresu';
        return false;
    }
    if (!tekst.includes('@')) {
        document.getElementById('emailError').innerText = 'Nepravilna mail adresa';
        return false;
    }
    return true;
}


// Function to validate username
function validacija_kor_ime(tekst) {
    if (tekst.trim() === '' || tekst.includes(' ')) {
        document.getElementById('korImeError').innerText = 'Unesite validno korisničko ime';
        return false;
    }
    return true;
}

// Function to validate password
function validacija_lozinke(tekst) {
    if (tekst.trim() === '') {
        document.getElementById('lozinkaError').innerText = 'Unesite lozinku';
        return false;
    }
    return true;
}

// Function to validate password confirmation
function validacija_potvrde_lozinke(tekst1, tekst2) {
    if (tekst1 !== tekst2) {
        document.getElementById('potvrdaLozinkeError').innerText = 'Potvrda se ne slaže sa lozinkom';
        return false;
    }
    return true;
}

// Function to clear error messages
function clearErrors() {
    document.getElementById('imePrezimeError').innerText = '';
    document.getElementById('emailError').innerText = '';
    document.getElementById('korImeError').innerText = '';
    document.getElementById('lozinkaError').innerText = '';
    document.getElementById('potvrdaLozinkeError').innerText = '';
}

// Display all users in a table
function prikaziKorisnike() {
    let tabela =
        "<tr>" +
            "<td>Ime i prezime</td>" +
            "<td>E-mail</td>" +
            "<td>Korisničko ime</td>" +
            "<td>Uslovi registracije</td>" +
        "</tr>";

    korisnici.forEach(function(korisnik) {
        tabela += 
            "<tr style=\"background-color: " + (korisnik.uvjetPrihvacen ? 'white' : 'red') + ";\">" +
                "<td>" + korisnik.imePrezime + "</td>" +
                "<td>" + korisnik.email + "</td>" +
                "<td>" + korisnik.korIme + "</td>" +
                "<td>" + (korisnik.uvjetPrihvacen ? 'Prihvaćeno' : 'Nije prihvaćeno') + "</td>" +
            "</tr>";
    });

    document.getElementById('korisniciTabela').innerHTML = tabela;
}
</script>
</body>
</html>
///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

<html>
<head> 
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<link rel="stylesheet" type="text/css" href="style.css"> 
</head> 

<body>
	<div>
		<table>
			<tr>
				<td>
				<h2>Izbor Matematičke Operacije</h2>
				</td>
			</tr>
			
			<tr>
				<td>
				<p>Odaberi opciju:</p>
				</td>
			</tr>
			
			<tr>
				<td>
					<label><input type="radio" name="operacija" value="sabiranje">Sabiranje</label>
					<label><input type="radio" name="operacija" value="oduzimanje">Oduzimanje</label>
					<label><input type="radio" name="operacija" value="mnozenje">Množenje</label>
					<label><input type="radio" name="operacija" value="dijeljenje">Dijeljenje</label>
				</td>
			</tr>
			
			<tr>
				<td>
					<label>Prvi broj: <input type="number" id="broj1"></label>
					<br>
					<label>Drugi broj: <input type="number" id="broj2"></label>
				</td>
			</tr>
			
			<tr>
				<td>
				<button class="zelena" onClick="izvrsiOperaciju()">Izvrši operaciju</button>
				<button class="crvena" onClick="ponistiUnos()">Poništi</button>
				<button class="crna" onClick="izlaz()">Izlaz</button>
				</td>
			</tr>
			
			<tr>
				<td>
				<h3>Rezultat:</h3>
				<textarea id="rezultat" readonly></textarea>
				</td>
			</tr>
			
			<tr>
				<td>
				<p id="datum"></p>
				</td>
			</tr>
		</table>
	</div>
	
<script>
	function izvrsiOperaciju() {
		//prikupljanje vrijednosti
		var broj1 = document.getElementById("broj1").value;
		var broj2 = document.getElementById("broj2").value;
		var rezultatTextArea = document.getElementById("rezultat");
		
		//validacija za odabranu opciju 
		var operacija = document.querySelector('input[name="operacija"]:checked');
		if(!operacija) {
			alert("Morate odabrati prvo matematičku operaciju");
			return;
		}
		
		//validacija da li su oba broja unesena
		if(broj1 === "" || broj2 === "") {
			alert("Morate unijeti oba broja!");
			return;
		}
		
		//validacija da li su unijeti brojevi (ili slovo ili prazan znak)
		if(isNaN(broj1) || isNaN(broj2)) {
			alert("Morate unijeti brojeve!");
			return;
		}
		
		//pretvaranje unijetih vrijednosti u brojeve
		broj1 = parseFloat(broj1);
		broj2 = parseFloat(broj2);
		
		//izbor operacije
		var operacijaValue = operacija.value;
		var rezultat; 
		
		switch (operacijaValue) {
			case "sabiranje":
				rezultat = broj1 + broj2;
				break;
			
			case "oduzimanje":
				rezultat = broj1 - broj2;
				break;
				
			case "mnozenje":
				rezultat = broj1 * broj2;
				break;
				
			case "dijeljenje":
				if(broj2 === 0) {
					alert("Dijeljenje s nulom nije moguće!");
					return;
				}
				rezultat = broj1 / broj2;
				break;
				
			default:
				alert("Nepoznata operacija");
				return;
		}
		
		rezultatTextArea.value = "Rezultat: " + rezultat;
	}
	
	function updateDateTime() {
		document.getElementById("datum").innerText = new Date().toLocaleString();
	}

	setInterval(updateDateTime, 1000); // Ažurira svakih 1000 ms (1 sekunda)

	
	function ponistiUnos() {
        document.getElementById("broj1").value = "";
        document.getElementById("broj2").value = "";
        document.getElementById("rezultat").value = "";
	}
	
	function izlaz() {
		window.close();
	}
</script>
</body>
</html>


CSS:
body {
	font-family: Arial, sans-serif;
	padding: 20px;
	display: flex;
	align-items: center;
	justify-content: center;
	height: 100vh;
	margin: 0;
	background-color: #f4f4f4;
}

table {
	border: 1px solid #ccc;
	padding: 20px;
	background-color: #fff;
	border-radius: 8px;
}

td, th {
	padding: 10px;
}

label {
	display: inline-block;
	margin-bottom: 5px;
}

input[type="number"] {
	width: 100px;
}

textarea {
	width: 100%;
	height: 150px;
	margin-top: 10px;
}

.radio-group label{
	margin-right: 15px;
}

button {
	padding: 8px 15px;
	border: none;
	border-radius: 5px;
	cursor: pointer;
	margin-right: 10px;
	opacity: 0.8;
}

button.zelena{
	background-color: #4CAF50;
	color: white;
}

button.crvena{
	background-color: #f44336;
	color: white;
}

button.crna{
	background-color: #000;
	color: white;
}

#datum {
	margin-top: 20px;
	font-size: 14px;
	background-color: #fff;
	padding: 5px 10px;
	border: 1px solid #ccc;
	border-radius: 5px;
	box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
	width: fit-content;
}


/////////////////////////////////////////////////////////////////////////////////////////////////////////////////

