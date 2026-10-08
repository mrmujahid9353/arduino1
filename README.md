void setup() {
  Serial.begin(9600);

  // Configure Timer1: Normal mode, no prescaler (Runs at full CPU clock)
  TCCR1A = 0;
  TCCR1B = 0;
}

void loop() {
  unsigned int cycles;

  // Reset Timer1 to 0
  TCNT1 = 0;

  // Start Timer1 with no prescaler (CS10 bit set to 1)
  TCCR1B = (1 << CS10);

  // --- START OF CODE TO MEASURE --- 
  //digitalWrite(13, HIGH);
  digitalWrite(14, LOW);
  // --- END OF CODE TO MEASURE ---

  // Stop the timer immediately by clearing the clock select bits
  TCCR1B = 0;

  // Read the elapsed clock cycles
  cycles = TCNT1;

  // Check for timer overhead (the time it takes to start/stop the timer, approx 4 cycles)
  if (cycles >= 4) {
    cycles -= 4;
  } else {
    cycles = 0;
  }

  // Print results
  Serial.print("Clock Cycles: ");
  Serial.println(cycles);

  Serial.print("Execution Time: ");
  Serial.print(cycles * 62.5);
  Serial.println(" ns");

  delay(5000); // Wait 5 seconds before repeating
}
