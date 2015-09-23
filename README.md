# -aktun-Run
Videojuego
--------------------------------------------------------------------------------------------------------------------------------
RebotePelotaPicos
using UnityEngine;
using System.Collections;

public class RebotePelotasPico : MonoBehaviour {
    public float speed = -0.5f;
	// Use this for initialization
	void Start () {
	
	}
	
	// Update is called once per frame
	void Update () {
        transform.Translate(-(speed * Time.deltaTime), 0, 0);
        speed += 0.01f;
	}
}
-----------------------------------------------------------------------------------------------------------------------------
RecolecciónMonedas
public class MonedaRecoleccion : MonoBehaviour
{

    public int puntosAgregar;
    public AudioSource SonidoMoneda;

    void OnTriggerEnter2D(Collider2D coll)
    {
        Puntuaje.SumarPuntos(puntosAgregar);

        SonidoMoneda.Play();
        Destroy(gameObject);


    }
}
-----------------------------------------------------------------------------------------------------------------------------
Puntuaje
using UnityEngine;
using System.Collections;
using UnityEngine.UI;

public class Puntuaje : MonoBehaviour
{


    public static int puntuaje;

    Text text;

    void Start()
    {
        text = GetComponent<Text>();
        puntuaje = 0;
    }

    void Update()
    {
        if (puntuaje < 0)
            puntuaje = 0;

        text.text = "" + puntuaje;
    }

    public static void SumarPuntos(int puntosAgregar)
    {
        puntuaje += puntosAgregar;
    }

}
-----------------------------------------------------------------------------------------------------------------------------
RotacionMonedas
using UnityEngine;
using System.Collections;

public class RotaciónMoneda : MonoBehaviour {
    public float speed = 75.7f;
	// Use this for initialization
	void Start () {
	
	}
	
	// Update is called once per frame
	void Update () {
        this.transform.Rotate(0, speed * Time.deltaTime, 0);
	}
}
-----------------------------------------------------------------------------------------------------------------------------
MovimientoPersonaje
using UnityEngine;
using System.Collections;
using UnityEngine.UI;

public class Personaje : MonoBehaviour {

    public float fuerzaSalto = 100f;

    //Validar si el personaje salta desde el suelo
    private bool enSuelo = true;
    public Transform comprobadorSuelo;
    private float comprobadorRadio = 0.07f;
    public LayerMask mascaraSuelo;

    //var para accesora de la animacion
    private Animator animator;

    //var detectar si esta corriendo
    private bool corriendo = false; //Inicia false porque se tiene que dar clic/touch para empezar
                                    //private bool corriendo = true;	//Inicia true para no dar clic/touch para empezar
                                    //var velocidad correr
    public float velocidad = 1f;

    //public static int puntaje = 0;
  
    //public Text textoPuntaje;

    //Se llama solo 1 vez, al crear el objeto, aqui se deben de llamar los componentes del juego
    void Awake() {
        animator = GetComponent<Animator>();
    }

    // Use this for initialization
    void Start() {

    }

    // Actualizacion de motor fisica, NO definido por frames sino por constantes
    void FixedUpdate() {
        if (corriendo) {
            GetComponent<Rigidbody2D>().velocity = new Vector2(velocidad, GetComponent<Rigidbody2D>().velocity.y);

        }
        //animator.SetFloat ("velX", GetComponent<Rigidbody2D>().velocity.x);
        enSuelo = Physics2D.OverlapCircle(comprobadorSuelo.position, comprobadorRadio, mascaraSuelo);
        animator.SetBool("enSuelo", enSuelo);
    }

    // Update is called once per frame
    void Update() {
        velocidad += 0.005f;

        if (Input.GetMouseButtonDown(0)) {      //Valida que sea haya dado clic o touch: Input.GetMouseButtonDown(0)
            if (corriendo) {    //saltar SI se puede
                if (enSuelo && Input.GetMouseButtonDown(0)) {
                    GetComponent<Rigidbody2D>().AddForce(new Vector2(0, fuerzaSalto));
                }
            }
            else {
                corriendo = true;
                //NotificationCenter.DefaultCenter().PostNotification(this, "PersonajeEmpiezaACorrer");
                Debug.Log("PersonajeEmpiezaACorrer");
            }
        }

    }

    /*void OnTriggerEnter2D (Collision2D col)
    {
        if (col.GetComponent<PlayerController>() == 
        {
            Destroy(col.gameObject);
            puntaje += 100;
            asignarPuntaje();
        }
        else if (col.gameObject.name == "Moneda")
        {
            Destroy(col.gameObject);
            puntaje += 300;
            asignarPuntaje();
        }
    }
    

	void asignarPuntaje(){
		textoPuntaje.text = "Score: " + puntaje;
	}*/

}
