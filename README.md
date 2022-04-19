# card-memory-game
# kart hafıza oyunu
Unity game engine kullanarak c# üzerinden projeyi gerçekleştirdim.
Ui olarak internet üzerinden bulduğum fotoğrafları kullandım. 
Oyunda iki zorluk modu bulunuyor ve tıklama ve eşleştirmeleri tutuyor.

Oyunda 2 adet cs dosyası var 1. olan kartların kontrolü için diğer script ise oyun kontrolü için.
```sh
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
public class kartDonme : MonoBehaviour
{
    private SpriteRenderer rend;
    [SerializeField]
    private Sprite faceSprite, backSprite;
    GameObject manager;
    public bool coroutineAllowed, facedUp;
    public bool ahtapot = false;
    public bool balik = false;
    public bool denizAnasi = false;
    public bool istiridye = false;
    public bool kaplumbaga = false;
    public bool yengec = false;
    public string first, second, temp2;
    public bool donmeControl;

    // Start is called before the first frame update
    void Start()
    {
        manager = GameObject.FindGameObjectWithTag("camera");
        rend = GetComponent<SpriteRenderer>();
        rend.sprite = backSprite;
        coroutineAllowed = true;
        facedUp = false;
    }
    private void Update()
    {
        donme();

    }
    private void OnMouseDown()
    {
        if (coroutineAllowed)
        {
            StartCoroutine(RotateCard());
        }

        hahayt();

    }
    public void hahayt()
    {
        temp2 = this.gameObject.tag;
        if (manager.gameObject.GetComponent<gameManager>().temp == "1")
        {
            manager.gameObject.GetComponent<gameManager>().temp = temp2;

        }
        else if (manager.gameObject.GetComponent<gameManager>().temp != "1")
        {
            manager.gameObject.GetComponent<gameManager>().temp2 = temp2;
        }
    }
    public void donme()
    {
        if (donmeControl)
        {
            StartCoroutine(rotateback());
            donmeControl = false;
        }
    }
    private IEnumerator RotateCard()
    {
        coroutineAllowed = false;

        if (!facedUp)
        {
            for (float i = 0f; i <= 180f; i += 10f)
            {
                transform.rotation = Quaternion.Euler(0f, i, 0f);
                if (i == 90f)
                {
                    rend.sprite = faceSprite;
                }

                yield return new WaitForSeconds(0.01f);
            }
        }

        else if (facedUp)
        {
            for (float i = 180f; i >= 0f; i -= 10f)
            {
                transform.rotation = Quaternion.Euler(0f, i, 0f);
                if (i == 90f)
                {
                    rend.sprite = backSprite;

                }
                yield return new WaitForSeconds(0.01f);
            }
        }

        coroutineAllowed = true;

        facedUp = !facedUp;
    }
    public IEnumerator rotateback()
    {
        
            yield return new WaitForSecondsRealtime(1f);

            for (float i = 180f; i >= 0f; i -= 10f)
            {
                transform.rotation = Quaternion.Euler(0f, i, 0f);
                if (i == 90f)
                {
                    rend.sprite = backSprite;
                }
            facedUp = !facedUp;
                yield return new WaitForSeconds(0.01f);
            }
        

    }
}

```

```sh
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;
public class gameManager : MonoBehaviour
{
    int randomint = 0;
    public GameObject[] spawnyeri;
    public List<GameObject> kartlar;
    public string first;
    public string temp;
    public string second;
    public string temp2 = null;
    public GameObject currentCard;
    public GameObject currentCard2;
    public float sayac = 0;
    public static int zorluk;
    public Text mytext;
    public Text mytext2;
    public int hamleSayisi;
    public int eslestirmeSayisi;

    void Start()
    {
        temp = "1";
        for (int i = 0; i < zorluk; i++)
        {

            Instantiate(kartlar[i], spawnyeri[randomint].GetComponent<Transform>().position, Quaternion.identity);
            randomint++;
        }

    }

    void Update()
    {
        mousekontrol();
        currentCard = GameObject.FindGameObjectWithTag(temp);
        currentCard2 = GameObject.FindGameObjectWithTag(temp2);
        if (temp == temp2)
        {
            eslestirmeSayisi++;
            mytext2.text = "Eslestirme Sayisi    : " + eslestirmeSayisi;
            temp = "1";
            temp2 = "2";
            
        }
        else
        {

            currentCard.gameObject.GetComponent<kartDonme>().donmeControl = true;
            currentCard2.gameObject.GetComponent<kartDonme>().donmeControl = true;
            temp = "1";
            temp2 = "2";
            currentCard = null;
            currentCard2 = null;

        }
        if(zorluk == 8)
        {
            if (eslestirmeSayisi == 4)
            {
                SceneManager.LoadScene(0);
            }
        }
    }
    public void mousekontrol()
    {
        if (Input.GetMouseButtonDown(0))
        {
            hamleSayisi++;
            mytext.text = "hamlesayisi   : " + hamleSayisi;
        }
    }

   
    public void kolay()
    {
        zorluk = 8;
        SceneManager.LoadScene(1);
    }
    public void zor()
    {
        zorluk = 12;
        SceneManager.LoadScene(1);
    }
}

```

<p><img align = "center" src = "https://github.com/OguzhanUgur1/card-memory-game/blob/main/2022-04-19-20-39-06.gif " width = "1080" height = "600" /></p>





