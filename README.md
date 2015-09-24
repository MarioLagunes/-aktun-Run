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


-----------------------------------------------------------------------------------------------------------------------------

using UnityEngine;
using System.Collections;

public class Scroll : MonoBehaviour {

	public float velocidad = 0.5f;

	// Use this for initialization
	void Start () {
	
	}
	
	// Update is called once per frame
	void Update () {
		if(SeguirPersonaje.personajeMuerto){		
			velocidad = 0.01f;
		}
		Vector2 offset = new Vector2(Time.time * velocidad, 0);
		GetComponent<Renderer>().material.mainTextureOffset = offset;
	}
}

-----------------------------------------------------------------------------------------------------------------------------

using UnityEngine;
using System.Collections;

public class Rotacion : MonoBehaviour {

    public float speed = 75f;

	void Start () {
	
	}

	void Update () {
        transform.Rotate(0, 0, speed * Time.deltaTime);
	
	}
}
-----------------------------------------------------------------------------------------------------------------------------
using UnityEngine;
using System.Collections;
using UnityEngine.UI;

public class RestarVidas : MonoBehaviour {

	public static int vidas = 3;
	private GameObject vida1,vida2,vida3;

	// Use this for initialization
	void Start () {
		//vidas = 3;
		vida1 = GameObject.Find("Vida1");
		vida2 = GameObject.Find("Vida2");
		vida3 = GameObject.Find("Vida3");
	}
	
	// Update is called once per frame
	void Update () {	
		if(vidas>=3){
			vida1.GetComponent<Image>().color = new Color32(255,255,255,240);
			vida2.GetComponent<Image>().color = new Color32(255,255,255,240);
			vida3.GetComponent<Image>().color = new Color32(255,255,255,240);
		}
		else if(vidas==2){
			vida1.GetComponent<Image>().color = new Color32(255,255,255,240);
			vida2.GetComponent<Image>().color = new Color32(255,255,255,240);
			vida3.GetComponent<Image>().color = new Color32(0,0,0,240);
		}
		else if(vidas==1){
			vida1.GetComponent<Image>().color = new Color32(255,255,255,240);
			vida2.GetComponent<Image>().color = new Color32(0,0,0,240);
			vida3.GetComponent<Image>().color = new Color32(0,0,0,240);
		}
		if(vidas<1){
			vida1.GetComponent<Image>().color = new Color32(0,0,0,240);
			vida2.GetComponent<Image>().color = new Color32(0,0,0,240);
			vida3.GetComponent<Image>().color = new Color32(0,0,0,240);
		}
	}
	
	public static void restarVida(int numero){
		vidas -= numero;
	}

	public static void agregarVida(int numero){
		if(vidas>3){
			Personaje.puntaje += 500*numero;
			vidas = 3;
		}
		else{
			vidas += numero;
			if(vidas>3){
				vidas = 3;
			}
		}
	}

	public static int getVidas(){
		return vidas;
	}

}
-----------------------------------------------------------------------------------------------------------------------------

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
using UnityEngine;
using System.Collections;
using UnityEngine.UI;

public class Progreso : MonoBehaviour {

	public GameObject[] suelos;
	private float escalaTotal;
	public Scrollbar barraProgreso;
	private GameObject personaje;
	private int posicionFinal;

	// Use this for initialization
	void Start () {
		for(int i=0; i<suelos.Length; i++){
			escalaTotal += suelos[i].transform.localScale.x;
			//print(suelos[i].transform.localScale.x);
		}escalaTotal -= 15;	// de tolerancia para teletransportacion
		personaje = GameObject.FindWithTag("Player");
		posicionFinal = (int) (escalaTotal/0.8f);
	}
	
	// Update is called once per frame
	void Update () {		
		actualizarProgreso();
	}

	void actualizarProgreso(){
		float posicionActual;
		posicionActual = (posicionFinal/2) + personaje.transform.position.x;
		if(personaje.transform.position.x >=0 ){
			posicionActual = personaje.transform.position.x + (posicionFinal/2);
		}
		barraProgreso.size = ((posicionActual*100)/posicionFinal) / 100;
		//print (((posicionActual*100)/posicionFinal) / 100);
	}

}

-----------------------------------------------------------------------------------------------------------------------------

using UnityEngine;
using System.Collections;

public class MovPicosAbajo : MonoBehaviour {
    public float speed = -0.5f;
	// Use this for initialization
	void Start () {
	
	}
	
	// Update is called once per frame
	void Update () {
        transform.Translate(0, speed * Time.deltaTime, 0);
	}
}
using UnityEngine;
using System.Collections;

public class MovPicosAbajo : MonoBehaviour {
    public float speed = -0.5f;
	// Use this for initialization
	void Start () {
	
	}
	
	// Update is called once per frame
	void Update () {
        transform.Translate(0, speed * Time.deltaTime, 0);
	}
}


-----------------------------------------------------------------------------------------------------------------------------

using UnityEngine;
using System.Collections;

public class MovPicosAbajo : MonoBehaviour {
    public float speed = -0.5f;
	// Use this for initialization
	void Start () {
	
	}
	
	
-----------------------------------------------------------------------------------------------------------------------------

using UnityEngine;
using System.Collections;

public class BloqueInicio : MonoBehaviour {

	private GameObject bloque;

	// Use this for initialization
	void Start () {
		bloque = GameObject.FindWithTag("BloqueInicial");
	}
	
	// Update is called once per frame
	void Update () {
		StartCoroutine(esperarDestruir(0.7F));
	}

	IEnumerator esperarDestruir(float delay)
	{
		yield return new WaitForSeconds(delay);
		Destroy(bloque.gameObject);
	}

}
	// Update is called once per frame
	void Update () {
        transform.Translate(0, speed * Time.deltaTime, 0);
	}
}
