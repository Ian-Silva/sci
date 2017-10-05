# sci


clear
// define tempo de jogo e numero de jogador
tempo = 10;nj = 3;
// define campo
//aa = 0:20;
ab = linspace(12,12,21);ac = linspace(0,0,21);ad = linspace(0,0,12);ae = linspace(0,12,12);
af = linspace(20,20,12);ag = 17:20;ak = linspace(7.5,7.5,4);aka = linspace(4.5,4.5,4);agg = 0:3;
akk = linspace(7.5,7.5,4);akaka = linspace(4.5,4.5,4);b = [10,6];
// define posição e força de cada jogador vetor 3X1 para cada jogador
// as duas primeiras colunas são as posições cartesianas do jogador
// a terceira coluna a força do jogador
nj = 3;i = 1;pf_time1 = [];pf_time2 = [];
sorte1 = 5; sorte2 = 4;

while i<=nj
  /// time 1
  // posição x no plano cartesiano
  aa = 10+10 .*rand(1,1);
  // posição y no plano cartesiano
  bb = 12 .*rand(1,1);
  // força do jogador
  ff = rand(1,1);
// concatena em t1 as definições jogador i time 1
  t1 = [aa,bb,ff];
  // concatena em pf_time1 as definições de cada jogador i  time 1
    pf_time1 = [pf_time1;t1];
  
  // time 2
    // posição x no plano cartesiano
  aaa = 10 .*rand(1,1);
 // posição y no plano cartesiano
  bbb = 12 .*rand(1,1);
  // força do jogador
  fff = rand(1,1);
// concatena em t2 as definições jogador i time 2
  t2 = [aaa,bbb,fff];
// concatena em pf_time2 as definições de cada jogador i  time 2
  pf_time2 = [pf_time2;t2];
  i = i+1;
end;
//% define distância inicial da bola(b) ao centro do gol a([20,6]) e b([0,6]) 
dbola_gola = b-[20,6];mdba = sqrt(dbola_gola*dbola_gola');
dbola_golb = b-[0,6];mdbb = sqrt(dbola_golb*dbola_golb');
//% contador j passos
j = 1;
// desenha campo com jogadores

plot(pf_time1(:,1),pf_time1(:,2),"k*",pf_time2(:,1),pf_time2(:,2),"r*",b(1),b(2),"ko",aa,ab,"r",aa,ac,"r",ad,ae,"r",af,ae,"r",ag,ak,"r",ag,aka,"r",agg,akk,"r",agg,akaka,"r");
//inicia jogo com j passos
while j<tempo
  
  
// condição de bola fora do gol
  if mdba>1 & mdbb>1 then
//········.....................................................................
    //% inicia vetores para acumular as informações de cada jogador
    nova_posicao_jogadores = [];  d_ac = [];  dbb_ac = [];  nova_posicao_jogadoresb = [];  djg_ac = [];  djg_ac2 = [];
    
    //% cria heuristica para todos (tj) jogadores
    k = 1;
    while k<=nj
    
    sorte1 = rand(1);
    sorte2 = rand(1);
//......................................................................

      //% posição e velocidade do jogador k time 1
      j_i = pf_time1(k,:)
      //% distância vetorial do jogador k em relação a bola
      dir = b-[j_i(1) j_i(2)];
      //% distância escalar do jogador i da bola
      mdir = sqrt(dir*dir');  d_ac = [d_ac;mdir];
      //% vetor unitario da direção
      dir_uni = dir/mdir;
      //% define distância do jogador ao seu gol
      djgol = [20,6]-[j_i(1),j_i(2)];  mdjgol = sqrt(djgol*djgol');  djg_ac = [djg_ac;mdjgol];

      //% posição instantanea apos um passo
      pja_inst = j_i + mtlb_double(2)*[dir_uni,0];  nova_posicao_jogadores = [nova_posicao_jogadores;pja_inst];

//...............................................................

      //% posição e velocidade do jogador k time 2
      ji_t2 = pf_time2(k,:);
      // distância vetorial do jogador k em relação a bola
      dirb = b-[ji_t2(1),ji_t2(2)];
      //distância escalar do jogador k da bola e vetor unitario de direção
      mdirb = sqrt(dirb*dirb');  dbb_ac = [dbb_ac;mdirb];  dirbb_uni = dirb/mdirb;
      // posição instantanea apos um passo
      // do jogador i com velocidade bb(3) em relação a bola
      djgol2 = [0,6]-[ji_t2(1),ji_t2(2)];  mdjgol2 = sqrt(djgol2*djgol2');  djg_ac2 = [djg_ac2;mdjgol2];
      // função que fornece saída para as duas entradas mdjgol(distancia do jogador a seu gol) e m_dir (distancia do jogador ate a bol
      pjf_inst = ji_t2 + mtlb_double(2)*[dirbb_uni,0];  nova_posicao_jogadoresb = [nova_posicao_jogadoresb;pjf_inst];
      k = k+1;
    end;
  
    //% Encontra quem esta mais proximo da bola  ?
    
      d_bola = min(d_ac);    db_bola = min(dbb_ac);
    //% definine o limite da distancia para chutar
    if (d_bola < 1.1) | (db_bola < 1.1) then
      //% se os dois passam pelo limite e jogador do time 1 mais veloz que jogador do time 2, o jogador time1 chuta
      
      if((sorte1 > sorte2) & (d_bola < 2.1))
             //% calcula vetos direção do gol 1
        dir_chute = [20,6]-b;  mchute = sqrt(dir_chute*dir_chute');  dir_unit_chute = dir_chute/mchute;
        //% calcula proxima posição da bola apos chute com força contante de 2
        b = (b + 2*dir_unit_chute);  plot(b(1),b(2),"g*")
        
    else
        
        
        
        if((sorte2 > sorte1) & (db_bola < 2.1))   
             //% calcula vetos direção do gol 1
        dir_chute = [20,6]-b;  mchute = sqrt(dir_chute*dir_chute');  dir_unit_chute = dir_chute/mchute;
        //% calcula proxima posição da bola apos chute com força contante de 2
        b = (b + 2*dir_unit_chute);  plot(b(1),b(2),"g*")
        
    else
        
        
          
      if (d_bola > db_bola) then
        //% calcula vetos direção do gol 1
        dir_chute = [20,6]-b;  mchute = sqrt(dir_chute*dir_chute');  dir_unit_chute = dir_chute/mchute;
        //% calcula proxima posição da bola apos chute com força contante de 2
        b = (b + 2*dir_unit_chute);  plot(b(1),b(2),"g*")
      else
        //% calcula proxima posição da bola apos chute com força contante de 2
        dir_chute = [0,6]-b;  mchute = sqrt(dir_chute*dir_chute');  dir_unit_chute = dir_chute/mchute;
        //% calcula proxima posição da bola apos chute com força contante de 2
        b = (b + 2*dir_unit_chute);  plot(b(1),b(2),"b*")
      end;
      //% calcula nova distancia da bola ao gol dos dois times
      dbola_gola = (b-[20,6]);  mdba = sqrt(dbola_gola*dbola_gola');  dbola_golb = (b-[0,6]);  mdbb = sqrt(dbola_golb*dbola_golb');
    end;
    //% atribui novas posições na variavel pf_time
    pf_time1 = nova_posicao_jogadores;  pf_time2 = nova_posicao_jogadoresb;
    clf()
    
    plot(b(1),b(2),'g*',nova_posicao_jogadores(:,1),nova_posicao_jogadores(:,2),"k.",nova_posicao_jogadoresb(:,1),nova_posicao_jogadoresb(:,2),"r.",aa,ab,"r",aa,ac,"r",ad,ae,"r",af,ae,"r",ag,ak,"r",ag,aka,"r",agg,akk,"r",agg,akaka,"r");
    
    
    
    j = j+1;
  else
    j = tempo;
  end;
end;
