// Pawel Masluch

#include<iostream>
#include<queue>


using namespace std;




const int UNDEF = -1; // wartosc niezdefiniowana

const int INF = 1000099; // nieskonczonosc




char T[6][6]; // plansza

int shortest_path[6][6]; // tablica najkrotszych sciezek do kazdego pola planszy

// tablice reprezentujace dozwolone ruchy na planszy: (i,j) -> ( i + x[k],  j + y[k] ),  dla k=0,1,2,3
int x[4] = { -1, 0, 0, 1 };
int y[4] = { 0, 1, -1, 0 };

bool visited[6][6]; // czy dla danego pola wykonywalismy relaksowanie w algorytmie Dijkstry

int last[6][6]; // last[i][j] = k oznacza, ze tuz przed polem (i,j), na najkrotszej sciezce do pola (i,j), jest pole ( i - x[k],  j - y[k] )

char path[6][6]; // znaleziona najkrotsza sciezka z lewego gornego do prawego dolnego rogu planszy




void input() // wczytanie planszy
{
    for(int i=0; i<=5; ++i)
    {
        for(int j=0; j<=5; ++j)
        {
            cin >> T[i][j];
        }
    }
}


void init() // inicjacja tablic 'last', 'shortest_path', 'path'
{
    for(int i=0; i<=5; ++i)
    {
        for(int j=0; j<=5; ++j)
        {
        	visited[i][j]= false; // dla pola (i,j) nie wykonywalismy dotychczas relaksowania w algorytmie Dijkstry
        	
            last[i][j] = UNDEF; // na razie nie wiadomo, jaki jest przedostatni wierzcholek na najkrotszej sciezce do pola (i,j)
            
			shortest_path[i][j] = INF; // na razie najkrotsza sciezka do pola (i,j) wynosi nieskonczonosc
            
			path[i][j] = ' '; // na razie pole (i,j) nie jest na najkrotszej sciezce z lewego gornego do prawego dolnego rogu planszy
        }
    }

    shortest_path[0][0] = 0; // najkrotsza sciezka do pola startowego wynosi 0
    
    path[0][0] = path[5][5] = '0'; // lewy gorny i prawy dolny rog planszy sa na najkrotszej sciezce z lewego gornego do prawego dolnego rogu planszy

}


int cost(int i, int j) // koszt wejscia na pole (i,j) planszy
{
    return T[i][j] - '0';
}


int VERTEX(int i, int j) // wyznaczenie numeru wierzcholka reprezentujacego pole (i,j) planszy
{
    return i*6 + j;
}


int ROW(int u) // wyznaczenie numeru wiersza dla pola reprezentowanego przez wierzcholek 'u'
{
    return u / 6;
}


int COLUMN(int u) // wyznaczenie numeru kolumny dla pola reprezentowanego przez wierzcholek 'u'
{
    return u % 6;
}


bool inside(int i, int j) // czy pole (i,j) miesci sie w planszy
{
	return ( 0 <= i  &&  i <= 5 )  &&  ( 0 <= j  &&  j <= 5 );
}


void Dijkstra() // znalezienie dlugosci najkrotszych sciezek do kazdego pola planszy 
{
    priority_queue <   pair < int, int >   >    PQ; // kolejka priorytetowa par postaci (-1*koszt_dojscia_do wierzcholka_u, wierzcholek_u)

    PQ.push(   make_pair( 0, 0 )   ); // wrzucamy do kolejki (-1) * koszt dojscia do wierzcholka startowego (0) rowny -0 oraz wierzcholek startowy (0) 


    int row_u, col_u, row_v, col_v, u, v;

    while( !PQ.empty() ) // dopoki kolejka nie jest pusta
    {
        u = PQ.top().second; // wyciagamy z kolejki wierzcholek o najmniejszym koszcie dojscia do niego
        PQ.pop();
        
        // obliczenie numeru wiersza i kolumny dla pola reprezentowanego przez wierzcholek u
        row_u = ROW(u);
        col_u = COLUMN(u);
		
		if( visited[row_u][col_u] == false ) // jesli dla pola (row_u, col_u) nie wykonywalismy dotychczas relaksowania w algorytmie Dijkstry
		{
			for(int i=0; i<=3; ++i) // dla kazdego sasiada wierzcholka u
	        {
	        	// obliczenie numeru wiersza i kolumny dla sasiada wierzcholka u (czyli wierzcholka v)
	            row_v = row_u + x[i];
	            col_v = col_u + y[i];
	            
	            if( inside( row_v, col_v ) == true ) // jesli sasiad wierzcholka u jest na planszy
	            {
	            	v = VERTEX( row_v, col_v ); // dla pola (row_v, col_v) obliczamy numer wierzcholka v, reprezentujacego to pole
	
		            if( shortest_path[row_u][col_u] + cost( row_v, col_v ) < shortest_path[row_v][col_v] ) // relaksacja
		            {
		                shortest_path[row_v][col_v] = shortest_path[row_u][col_u] + cost( row_v, col_v ); // koszt nowej najkrotszej sciezki do pola (row_v, col_v)
		                
						last[row_v][col_v] = i; // przedostatnie pole na najkrotszej sciezce do pola (row_v, col_v) to pole ( row_v - x[i], col_v - y[i] )  
		
		                PQ.push(   make_pair( -shortest_path[row_v][col_v], v )   );
		            }	
				}
	        }
			
			visited[row_u][col_u] = true; // dla pola (row_u, col_u) wlasnie wykonalismy relaksowanie w algorytmie Dijkstry
		}
    }
}


void output() // wypisywanie najkrotszej sciezki z lewego gornego do prawego dolnego rogu planszy 
{
	/* 
		aktualnie:
		u - numer ostaniego wierzcholka na najkrotszej sciezce z lewego gornego do prawego dolnego pola planszy,
	   	row_u, col_u - wspolrzedne pola reprezentowanego przez wierzcholek u
	*/
    int u = 35, row_u = 5, col_u = 5;
    
    int dx, dy;
    
    while( u > 0 )
    {
    	// o ile zmniejszy sie wiersz i kolumna 
    	dx = x[ last[row_u][col_u] ];
    	dy = y[ last[row_u][col_u] ];
    	
    	// wspolrzedne nowego pola
        row_u -= dx;
        col_u -= dy;

        path[row_u][col_u] = T[row_u][col_u]; // nowe pole znajduje sie na najkrotszej sciezce z lewego gornego do prawego dolnego pola planszy

        u = VERTEX( row_u, col_u ); // tworzymy numer wierzcholka dla pola (row_u, col_u) 
    }
    
    
    // wypisywanie najkrotszej sciezki z lewego gornego do prawego dolnego pola planszy
    
    cout << endl;

    for(int i=0; i<=5; ++i)
    {
        for(int j=0; j<=5; ++j)
        {
            cout << path[i][j];
        }
        
        cout << endl;
    }

    cout << endl;
}


int main()
{
    input();

    init();

    Dijkstra();

    output();

    return 0;
}
