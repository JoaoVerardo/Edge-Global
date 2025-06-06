# Edge-Global
Sofia Bomeny - 563270
Guilherme Moura Badia - 561568
João Victor Santos Verardo - 563700
------------------------------------------------------------------------
nosso problema é relacionado as enchentes, que causam grandes prejuízos e colocam vidas em risco. Nesse quesito, a falta de monitoramento rápido dificulta ações preventivas. Por isso, desenvolvolvemos um sistema simples, acessível e eficiente para detectar níveis críticos de água e alertar com antecedência.
Utilizando um sensor ultrassônico, o sistema mede a distância até o nível da água. Quando a distância indica risco, ele aciona alertas visuais e sonoros, permitindo que as pessoas ajam rapidamente.
Como funciona
-O sistema é composto por:
-Arduino Uno, que controla tudo;
-Sensor HC-SR04, que mede a distância até a água;
-LEDs (verde e vermelho), que indicam nível seguro ou alerta;
-Buzzer, que emite um som em caso de risco.
-placa

link projeto tinkercad: https://www.tinkercad.com/things/gEHYZblZx79-mighty-robo/editel?returnTo=https%3A%2F%2Fwww.tinkercad.com%2Fdashboard&sharecode=2l-qY9z5EfOhcAJrMMatA0QLKcdfNjQvSHhA2YSXjTs

-----------------------------------------------------------------------

CODIGO EXPLICATIVO PROJETO

// Definindo os pinos utilizados
const int sensorPin = 7;         // Pino do sensor ultrassônico (pino único de sinal)
const int ledVerde = 6;          // LED verde (nível de água seguro)
const int ledVermelho = 5;       // LED vermelho (nível de água crítico)
const int buzzer = 4;            // Buzzer para alarme sonoro

void setup() {
  // Configurando os pinos como saída ou entrada
  pinMode(sensorPin, OUTPUT);       // O sensor inicialmente envia um pulso (modo OUTPUT)
  pinMode(ledVerde, OUTPUT);        // LED verde configurado como saída
  pinMode(ledVermelho, OUTPUT);     // LED vermelho configurado como saída
  pinMode(buzzer, OUTPUT);          // Buzzer configurado como saída

  // Inicia a comunicação serial (opcional, útil para monitorar as leituras)
  Serial.begin(9600);
}

void loop() {
  long duration;      // Variável para armazenar o tempo do pulso (em microssegundos)
  float distancia;    // Variável para armazenar a distância calculada (em cm)

  // Envia um pulso ultrassônico curto
  pinMode(sensorPin, OUTPUT);       // Define o pino como saída para enviar o pulso
  digitalWrite(sensorPin, LOW);     // Garante que o pino está baixo
  delayMicroseconds(2);             // Espera 2 microssegundos
  digitalWrite(sensorPin, HIGH);    // Envia pulso alto
  delayMicroseconds(5);             // Mantém o pulso por 5 microssegundos
  digitalWrite(sensorPin, LOW);     // Finaliza o pulso

  // Agora escuta o eco retornando
  pinMode(sensorPin, INPUT);        // Muda o pino para entrada
  duration = pulseIn(sensorPin, HIGH);    // Mede o tempo do eco (em microssegundos)

  // Converte o tempo em distância (fórmula baseada na velocidade do som)
  distancia = duration / 58.2;     // Distância em cm (aproximadamente)

  // Exibe os valores no monitor serial (para teste e monitoramento)
  Serial.print("Duracao: ");
  Serial.print(duration);
  Serial.print(" us, Distancia: ");
  Serial.print(distancia, 1);
  Serial.println(" cm");

  // Verifica o nível da água com base na distância
  if (distancia < 100.0) {  // ALERTA! Nível de água crítico (menos de 1 metro)
    digitalWrite(ledVerde, HIGH);      // Acende o LED verde (pode sinalizar sistema funcionando)
    digitalWrite(ledVermelho, LOW);    // Garante que o vermelho esteja apagado
    noTone(buzzer);                    // Garante que o buzzer esteja desligado
  } else {
    // Situação de risco - acende LED vermelho e ativa alarme sonoro
    digitalWrite(ledVerde, LOW);       // Apaga o LED verde
    digitalWrite(ledVermelho, HIGH);   // Acende o LED vermelho (alerta visual)
    tone(buzzer, 1500);                // Toca o buzzer com frequência de 1500Hz (alerta sonoro)
  }

  delay(1000);  // Aguarda 1 segundo antes de fazer a próxima leitura
}

