# Taller_Referencias_paquetes_encapsulamiento

package biblioteca.modelo;
public class Libro {
    private String titulo, autor;
    private boolean disponible;
    public Libro(String titulo, String autor) {
        setTitulo(titulo); this.autor = autor; this.disponible = true;
    }
    public boolean estaDisponible() { return disponible; }
    public void prestar() { disponible = false; }
    public void devolver() { disponible = true; }
    public void mostrarInformacion() { System.out.println(titulo + " de " + autor + " | disponible: " + disponible); }
    public String getTitulo() { return titulo; }
    public String getAutor() { return autor; }
    public void setTitulo(String titulo) {
        if (titulo == null || titulo.trim().isEmpty()) System.out.println("Título inválido");
        else this.titulo = titulo;
    }
    public boolean mismoTitulo(Libro otro) {
        return otro != null && this.titulo.equals(otro.titulo);
    }
}

package biblioteca.personas;
import biblioteca.modelo.Libro;
public class Socio {
    private String nombre;
    private int librosPrestados;
    public Socio(String nombre) { this.nombre = nombre; this.librosPrestados = 0; }
    public int getLibrosPrestados() { return librosPrestados; }
    public void prestarLibro(Libro libro) {
        if (libro == null) System.out.println("No hay libro para prestar");
        else if (!libro.estaDisponible()) System.out.println(libro.getTitulo() + " no está disponible");
        else { libro.prestar(); librosPrestados++; System.out.println(nombre + " recibió " + libro.getTitulo()); }
    }
    public void devolverLibro(Libro libro) {
        if (libro != null && librosPrestados > 0) { libro.devolver(); librosPrestados--; }
    }
}


package biblioteca.app;
import biblioteca.modelo.Libro;
import biblioteca.personas.Socio;
public class Biblioteca {
    public static void main(String[] args) {
        Libro l1 = new Libro("Cien años de soledad", "García Márquez");
        l1.mostrarInformacion();
        Libro l2 = l1, l3 = new Libro("Cien años de soledad", "García Márquez");
        l2.prestar();
        System.out.println(l1.estaDisponible()); // false
        System.out.println(l3.estaDisponible()); // true
        System.out.println(l1 == l2); // true
        System.out.println(l1 == l3); // false
        
        Libro l4 = null;
        if (l4 == null) System.out.println("No hay libro");
        
        l1.setTitulo("");
        System.out.println(l1.getTitulo());
        Libro l5 = new Libro("", "Anónimo");
        System.out.println(l5.getTitulo());
        
        Socio camila = new Socio("Camila");
        camila.prestarLibro(l3); camila.prestarLibro(l1); camila.prestarLibro(null);
        System.out.println(camila.getLibrosPrestados());
        System.out.println(l3.estaDisponible());
    }
}

package aerolinea.modelo;
public class Vuelo {
    private String codigo, destino;
    private int sillasDisponibles;
    public Vuelo(String codigo, String destino, int sillas) {
        this.codigo = codigo; setDestino(destino);
        this.sillasDisponibles = Math.max(0, sillas);
    }
    public String getCodigo() { return codigo; }
    public String getDestino() { return destino; }
    public int getSillasDisponibles() { return sillasDisponibles; }
    public void setDestino(String destino) {
        if (destino != null && !destino.trim().isEmpty()) this.destino = destino;
    }
    public boolean reservarSilla() {
        if (sillasDisponibles > 0) { sillasDisponibles--; return true; }
        return false;
    }
}

package aerolinea.modelo;
public class Pasajero {
    private String nombre;
    private Vuelo vuelo;
    public Pasajero(String nombre) { this.nombre = nombre; this.vuelo = null; }
    public Vuelo getVuelo() { return vuelo; }
    public void reservar(Vuelo vuelo) {
        if (vuelo == null) return;
        if (vuelo.reservarSilla()) {
            this.vuelo = vuelo;
            System.out.println(nombre + " reservó en " + vuelo.getCodigo());
        } else {
            System.out.println(nombre + ": vuelo " + vuelo.getCodigo() + " lleno");
        }
    }
    public void mostrarReserva() {
        if (vuelo == null) System.out.println(nombre + ": sin reserva");
        else System.out.println(nombre + " viaja a " + vuelo.getDestino());
    }
}

package aerolinea.app;
import aerolinea.modelo.*;
public class Aerolinea {
    public static void main(String[] args) {
        Vuelo v = new Vuelo("AV-204", "Cartagena", 2);
        Pasajero p1 = new Pasajero("Sara"), p2 = new Pasajero("Tomás"), p3 = new Pasajero("Lina");
        p3.mostrarReserva();
        p1.reservar(v); p2.reservar(v); p3.reservar(v);
        System.out.println(v.getSillasDisponibles());
        v.setDestino("Santa Marta");
        p1.mostrarReserva(); p2.mostrarReserva(); p3.mostrarReserva();
        System.out.println(p1.getVuelo() == p2.getVuelo());
    }
}

1. Diferencia entre int y Libro: int guarda un número directamente, mientras que Libro guarda una referencia que apunta a un objeto creado en el programa.

2. ¿Qué pasa al prestar con l2? l2 y l1 apuntan al mismo libro, así que si se modifica usando uno, el cambio se ve en ambos. En cambio, l3 es otro libro diferente porque se creó con new.

3. ¿Por qué ocurre un NullPointerException? Ocurre cuando intentamos usar un objeto que tiene el valor null, es decir, que no apunta a ningún objeto. El error aparece cuando el programa se está ejecutando.

4. Diferencia entre modificar y asignar un parámetro: Si modificamos un objeto desde un método, el cambio también se ve fuera de él. Pero si creamos otro objeto y se lo asignamos al parámetro, la variable original sigue apuntando al objeto anterior.

5. ¿Cómo se protege la información de Libro? Se usa el método setTitulo() para evitar que el título quede vacío o sea null. Si ocurre eso, se le asigna el texto "Sin título" para que el objeto tenga un título válido.
