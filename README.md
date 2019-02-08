# GAME
Juego donde dos momias se arrojaran calaveras que aumentaran su velocidad gradualmente
craneo Craneo;
//jugador 1 y 2
Demonio P1, P2;
PImage fondo;
PImage calaca;
PImage [] momias= new PImage[2];
int Score1 =0;
int Score2 =0;
int index;

void setup() {
  size(1000, 740);
  Craneo = new craneo();
  P1 = new Demonio(100);
  P2 = new Demonio(900);
}

void draw(){
  
  
  fondo = loadImage("fondo.jpg");
  image(fondo, 0, 0);
  fill(0);
  textSize(20);
  text(Score1,20,40);
  text(Score1,width-20,40);
  

  moverPlayer1();
  moverPlayer2();


  P1.mostrar();
  P2.mostrar();
  for(int i =0; i<momias.length; i++){
  momias[i] = loadImage("momia"+i+".png");}
  int index = int(random(0,momias.length));
  calaca = loadImage("calaca_1.png");

  Craneo.mover();
  //colicion entre la el demonio y la calavera
  calacaVSdemonio(P1);
  calacaVSdemonio(P2);

  Craneo.mostrar();
  // si el demonio se deja hacer gol, se repite
  if (Craneo.GOL())Craneo= new craneo();
  
}

// creamos la clase demonio que sera nuestro jugador
class Demonio {

  PVector pos;
  //radio en X y Radio en Y ya que tomaremos la mitad de referencia
  int ry=5;
  int rx=1;
  // (el parametro) que columna aparece cada demonio
  Demonio(int D_columna) {
    // estara en la fila de la del medio
    pos = new PVector(D_columna, height/2);
  }
  //imprime el demonio
  void mostrar() {

    //rectMode(RADIUS);
    //el demonio estara (en la pos indicada)
  
    //rect( pos.x, pos.y, ry, rx);
  }
  
}



class craneo {
  //posicion (tiene valores X y Y)
  PVector pos;
  //(se suma a la posicion para que esta cambie)
  PVector vel;
  //radio del craneo 
  int radio = 50;
  // cuando instancio la clase craneo, debe salir en la mitad de la pantalla
  craneo() {
    pos = new PVector(width/2, height/2);
    //un vector que que tiene X y Y aleatorios y magnitud de 1 
    vel =PVector.random2D();
    //se multiplica la velocidad por 6
    vel.mult(6);
  }
  // en lugar de la posicion mostrara el craneo
  void mostrar() {
    //para que el diametro cambie a radio 
    ellipseMode(CENTER);
    ellipse(pos.x, pos.y, radio, radio);
    fill(0);

    image(calaca, pos.x, pos.y, 100, 100);
  }

  // sumara la velocidad a la posicion
  void mover() {
    //add suma un vector a otro aqui sumamos la vel a pos
    pos.add(vel);
    // si la posicion de Y es menor que el radio o mayor que el alto de la pantalla se devuelve
    if (pos.y<100 || pos.y > height-100)vel.y*=-1.3;
  }
  // si le hacen un gol (verdadero o falso)
  boolean GOL() {
    //si la pos es menor que el tamaño <--- o menor que la ultima parte es verdadero o falso
    if (pos.x<-100 || pos.x> width+radio ) return true
   ;
    else return false;
    
    //mover
    
    void moverPlayer1() {
  // constrain tiene un valor, un minimo y un maximo
  // si es mayor que el minimo devuelve el minimo si es mayor que el macimo devuelve el maximo
  //esto es para regular el movimiento del demonio en Y
  P1.pos.y= constrain(mouseY, P1.ry, height-P1.ry);
}
void moverPlayer2() {

  P2.pos.y=constrain(height-mouseY, P2.ry, height-P1.ry);
}
// hacer rebotar la pelota
void calacaVSdemonio(Demonio P) {
  // calculando el punto donde chocan
  PVector colicion = new PVector (0, 0);
  // si el craneo esta a la derecha que la ultima columna, el punto de colicion estara sobre la ultima columna 
  if (Craneo.pos.x> P.pos.x-P.rx) colicion.x =P.pos.x+P.rx;
  // si esta a la izquierda, colicionara y se devolvere a la derecha
  else if (Craneo.pos.x< P.pos.x-P.rx)colicion.x =P.pos.x-P.rx;
  // si esta en la mitad tomara el valor del craneo
  else colicion.x =Craneo.pos.x;

  // si el craneo esta a la derecha que la ultima columna, el punto de colicion estara sobre la ultima columna 
  if (Craneo.pos.y> P.pos.y-P.ry) colicion.y =P.pos.y+P.ry;
  // si esta a la izquierda, colicionara y se devolvere a la derecha
  else if (Craneo.pos.y < P.pos.y-P.ry)colicion.y =P.pos.y-P.ry;
  // si esta en la mitad tomara el valor del craneo
  else colicion.y =Craneo.pos.y;
//ellipse(colicion.x,colicion.y,50,50);
 image(momias[1],colicion.x,colicion.y,100,100);
  float dist = PVector.dist(colicion, Craneo.pos);
  if (dist < Craneo.radio) {
    //ATAJA
    Craneo.vel.x*=-1.3;
  }
}
