# pwm-2
#define F_CPU  20000000
#include <avr/io.h>
#include <avr/interrupt.h>
#include <util/delay.h>
int duty=255,r,l;
int cliflag = 0;
void forward();
void backward();
void right();
void left();
void sright();
void sleft();
void PWM_init();
int main(void)
{
	DDRA=0b00001111;
	DDRC = 0xFF;
	DDRD = 0xFF;
	
	EIMSK |= (1<<INT1);
	EICRA |= (1<<ISC01); //falling edge generates interrupt request

	PORTD |=(1 << PIND1);
	
	
	PWM_init();
    OCR1A=(int)(duty*l); //setting PWM to required duty cycle based on direction
    OCR1B=(int)(duty*r);
	
	/* Replace with your application code */
	while (1)
	{
		if (cliflag==0)
		{
			sei();
		}
		else
		{
			cli();
			_delay_ms(1000);
			cliflag=0;
		}
	}
	/* Replace with your application code */
	OCR1A=(int)(duty*l); //setting PWM to 100% duty cycle
	OCR1B=(int)(duty*r);
	
	
}
void forward()
{
	PORTA=0b00000101;
		l=1;
	    r=1;
	
}
void backward()
{
	PORTA=0b00001010;
	l=1;
	r=2;
	
}
// OCR1A FOR LEFT AND OCR1B FOR RIGHT
void left()
{
	PORTA=0b00000001;
	l=0.2;
	r=0.8;
	
}

void right()
{
	PORTA=0b0000100;
	l=0.8;
	r=0.2;
	
}
void sleft()
{
	PORTA=0b00001001;
	l=0.5;
	r=0.8;
	
}
void sright()
{
	PORTA=0b00000110;
	l=0.8;
	r=0.5;
	
}
void PWM_init()
{
	TCCR1B |=(0<<WGM13)|(1<<WGM12)|(1<<CS10);
	DDRB |= (1<<PINB6)|(1<<PINB5);
}

ISR(INT1_vect)
{
	PORTC |= (1<<PINC0)|(1<<PINC1);
	_delay_ms(100);
	cliflag=1;
}
