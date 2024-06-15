#2D
![[ShootingGame.png]]
![[ShootingGame2.png]]
# Inspectors
![[ShootingGame_Inspectors.png]]

#### Player
![[ShootingGame_Player.png]]

#### GameManager
![[ShootingGame_GameManager.png]]

#### Wall
![[ShootingGame_Wall.png]]

#### PlayerBullet1
![[ShootingGame_PlayerBullet1.png]]

#### PlayerBullet2
![[ShootingGame_PlayerBullet2.png]]

#### EnemyA
![[ShootingGame_EnemyA.png]]

#### EnemyB
![[ShootingGame_EnemyB.png]]

#### EnemyC
![[ShootingGame_EnemyC.png]]

#### EnemyBulletA
![[ShootingGame_EnemyBulletA.png]]

#### EnemyBulletC
![[ShootingGame_EnemyBulletC.png]]

#### Boom
![[ShootingGame_Boom.png]]

#### Coin
![[ShootingGame_Coin.png]]

# Animators
#### Player
![[ShootingGame_Animator_Player.png]]

#### Power
![[ShootingGame_Power.png]]

#### BoomEffect
![[ShootingGame_BoomEffect.png]]

#### Boom
![[ShootingGame_Animator_Boom.png]]

#### Coin
![[ShootingGame_Animator_Coin.png]]

#### Power
![[ShootingGame_Animator_Power.png]]

#### BoomEffect
![[ShootingGame_Animator_BoomEffect.png]]

# Scripts
#### PlayerController
```cs
using System;
using System.Collections;
using System.Collections.Generic;
using Unity.Mathematics;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float _speed;
    public Vector2 minBoundary;
    public Vector2 maxBoundary;
    public Animator animator;
    public Transform _myTF;
    public Transform _bullet1;
    public Transform _bullet2;
    public float fireCooldown;
    private float cooldown;
    public int level;
    public int life;
    public SpriteRenderer _renderer;
    public float _invincibleTime;
    bool _invincible;

    private void Awake()
    {
        _speed = 5;
        minBoundary = new Vector2(-2.2f, -4.4f);
        maxBoundary = new Vector2(2.2f, 4.4f);
        animator = GetComponent<Animator>();
        fireCooldown = 0.1f;
    }

    // Update is called once per frame
    void Update()
    {
        float h = Input.GetAxisRaw("Horizontal");
        float v = Input.GetAxisRaw("Vertical");
        Vector3 nextPos = new Vector3(h, v, 0) * _speed * Time.deltaTime;
        _myTF.position = ClampVector(_myTF.position + nextPos, maxBoundary.x, maxBoundary.y);

        if ( Input.GetButtonDown("Horizontal") || Input.GetButtonUp("Horizontal") )
        {
            animator.SetInteger( "input", (int)h );
        }

        if ( Input.GetButton("Fire1") )
        {
            if (cooldown <= 0)
            {
                cooldown += fireCooldown;
                switch(level)
                {
                    case 0:
                        //Instantiate(_bullet1, _myTF.position, _myTF.rotation);
                        PoolManager.Spawn(_bullet1.gameObject, _myTF.position, _myTF.rotation);
                        break;
                    case 1:
                        //Instantiate(_bullet1, _myTF.position + new Vector3(-0.1f, 0, 0), _myTF.rotation);
                        //Instantiate(_bullet1, _myTF.position + new Vector3(0.1f, 0, 0), _myTF.rotation);
                        PoolManager.Spawn(_bullet1.gameObject, _myTF.position + new Vector3(-0.1f, 0, 0), _myTF.rotation);
                        PoolManager.Spawn(_bullet1.gameObject, _myTF.position + new Vector3(0.1f, 0, 0), _myTF.rotation);
                        break;
                    default:
                        //Instantiate(_bullet1, _myTF.position + new Vector3(-0.25f, 0, 0), _myTF.rotation);
                        //Instantiate(_bullet2, _myTF.position, _myTF.rotation);
                        //Instantiate(_bullet1, _myTF.position + new Vector3(0.25f, 0, 0), _myTF.rotation);
                        PoolManager.Spawn(_bullet1.gameObject, _myTF.position + new Vector3(-0.25f, 0, 0), _myTF.rotation);
                        PoolManager.Spawn(_bullet2.gameObject, _myTF.position, _myTF.rotation);
                        PoolManager.Spawn(_bullet1.gameObject, _myTF.position + new Vector3(0.25f, 0, 0), _myTF.rotation);
                        break;
                }
            }
            else
            {
                cooldown -= Time.deltaTime;
            }
        }
    } //Update()

    private void OnTriggerEnter2D(Collider2D collision)
    {
        switch(collision.tag)
        {
            case "EnemyBullet":
            case "Enemy":
                if (_invincible)
                {
                    return;
                }
                //Destroy(collision.gameObject);
                PoolManager.Despawn(collision.gameObject);
                gameObject.SetActive(false); //화면에서 사라짐
                GameManager.Instance.DecreaseLife();
                if ( GameManager.Instance.IsPlayerAlive() )
                {
                    Invoke("ShowPlayer", 2.0f);
                }
                break;
            case "Item":
                //Destroy(collision.gameObject);
                PoolManager.Despawn(collision.gameObject);
                ItemController controller = collision.GetComponent<ItemController>();
                if (controller == null )
                {
                    return;
                }

                switch(controller.type)
                {
                    case ItemType.coin:
                        GameManager.Instance.AddScore(1000);
                        break;
                    case ItemType.boom:
                        GameManager.Instance.Boom();
                        break;
                    case ItemType.power:
                        if (level < 2)
                        {
                            level++;
                        }
                        break;
                }
                break;
        }
    }

    void ShowPlayer()
    {
        life--;
        _myTF.position = new Vector3(0, -2.5f, 0);
        _invincible = true;
        gameObject.SetActive(true);
        StartCoroutine( DoBlink() );
    }

    IEnumerator DoBlink()
    {
        int count = (int)(_invincibleTime * 10);
        for (int i=0; i<count; i++)
        {
            _renderer.color = new Color(1f, 1f, 1f, 0.5f);
            yield return new WaitForSeconds(0.05f);
            _renderer.color = new Color(1f, 1f, 1f, 1f);
            yield return new WaitForSeconds(0.05f);
        }
        _invincible = false;
    }

    Vector3 ClampVector(Vector3 vector, float maxX, float maxY)
    {
        return new Vector3( Mathf.Clamp(vector.x, -maxX, maxX), Mathf.Clamp(vector.y, -maxY, maxY), 0 );
    }
}
```

#### GameManager
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;
using System;

public class GameManager : MonoBehaviour
{
    public static GameManager Instance;
    public Transform _myTF;
    public Transform _playerTF;
    public Transform[] arySpawn;
    public Transform[] aryEnemy;
    public float _spawnCooldown;
    private float _cooldown;

    private int life;
    private int score;
    public GameObject menuGameOver;

    public Action<int> onScoreChange;
    public Action<int> onLifeChange;

    public GameObject boomEffect;

    private void Start()
    {
        life = 3;
        score = 0;
        menuGameOver.SetActive(false);
        onScoreChange?.Invoke(score); // ? 연산자를 통한 NULL 값 체크 | NULL일 경우 ? 이후의 코드는 실행하지 않음.
        onLifeChange?.Invoke(life);
        boomEffect.SetActive(false);
    }

    private void Awake()
    {
        Instance = this;
    }

    private void Update()
    {
        if (_cooldown <= 0)
        {
            _cooldown += _spawnCooldown;
            int randSpawn = UnityEngine.Random.Range(0, arySpawn.Length);
            int randEnemy = UnityEngine.Random.Range(0, aryEnemy.Length);

            Vector2 direction = _playerTF.position - arySpawn[randSpawn].position;
            float angle = Mathf.Atan2(direction.y, direction.x) * Mathf.Rad2Deg;
            Quaternion rotation = Quaternion.identity;
            rotation.eulerAngles = new Vector3(0, 0, angle + 90);

            //Instantiate(aryEnemy[randEnemy], arySpawn[randSpawn].position, rotation);
            PoolManager.Spawn(aryEnemy[randEnemy].gameObject, arySpawn[randSpawn].position, rotation);
        }
        else
        {
            _cooldown -= Time.deltaTime;
        }
    }

    public void Boom()
    {
        boomEffect.SetActive(true);

        GameObject[] enemies = GameObject.FindGameObjectsWithTag("Enemy");
        foreach (GameObject obj in enemies)
        {
            EnemyController objController = obj.GetComponent<EnemyController>();
            if (objController)
            {
                objController.OnHit(100);
            }
        }

        GameObject[] bullets = GameObject.FindGameObjectsWithTag("EnemyBullet");
        foreach (GameObject obj in bullets)
        {
            //Destroy(obj);
            PoolManager.Despawn(obj.gameObject);
        }

        Invoke("EndBoom", 2);
    }

    void EndBoom()
    {
        boomEffect.SetActive(false);
    }

    public void AddScore(int score)
    {
        this.score += score;
        onScoreChange?.Invoke(this.score);
    }

    public void DecreaseLife()
    {
        life--;
        onLifeChange?.Invoke(life);

        if (life <= 0)
        {
            menuGameOver.SetActive(true);
        }
    }

    public bool IsPlayerAlive()
    {
        return life > 0;
    }

    public void ButtonAct_Restart()
    {
        SceneManager.LoadScene(0);
    }
}
```

#### WallController
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class WallController : MonoBehaviour
{
    private void OnTriggerEnter2D(Collider2D collision)
    {
        switch(collision.tag)
        {
            case "Bullet":
            case "Enemy":
            case "EnemyBullet":
                //Destroy(collision.gameObject);
                PoolManager.Despawn(collision.gameObject);
                break;
        }
    }
}
```

#### BulletController
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BulletController : MonoBehaviour
{
    public float _speed;
    public Rigidbody2D _rd;
    public Transform _myTF;
    public int damage;

    private void OnEnable()
    {
        _rd.AddForce(_myTF.up * _speed, ForceMode2D.Impulse);
    }
}
```

#### EnemyController
```cs
using System;
using System.Collections;
using System.Collections.Generic;
using TMPro.EditorUtilities;
using UnityEngine;

public class EnemyController : MonoBehaviour
{
    public float _speed;
    public Rigidbody2D _rd;
    public Transform _myTF;
    public int hp;
    public Sprite[] _sprite;
    public SpriteRenderer _renderer;
    public Transform _bulletTF;
    public float fireCooldown;

    private float cooldown;
    private int score;

    public Transform[] aryItem;

    private void OnEnable()
    {
        _rd.velocity = -_myTF.up * _speed;
        score = hp;
    }

    private void Update()
    {
        if (_bulletTF == null || !IsInScreen() || GameManager.Instance.IsPlayerAlive() == false )
        {
            return;
        }

        if (cooldown <= 0)
        {
            cooldown += fireCooldown;

            Transform _playerTF = GameManager.Instance._playerTF;
            Vector2 direction = _playerTF.position - _myTF.position;

            float angle = Mathf.Atan2(direction.y, direction.x) * Mathf.Rad2Deg;
            Quaternion rotation = Quaternion.identity;
            rotation.eulerAngles = new Vector3(0, 0, angle - 90);

            //Instantiate(_bulletTF, _myTF.position, rotation);
            PoolManager.Spawn(_bulletTF.gameObject, _myTF.position, rotation);
        }
        else
        {
            cooldown -= Time.deltaTime;
        }
    }

    bool IsInScreen()
    {
        if (_myTF.position.x > -2.2f && _myTF.position.x < 2.2f && _myTF.position.y > -4.4f && _myTF.position.x < 4.4f)
        {
            return true;
        }
        return false;
    }

    private void OnTriggerEnter2D(Collider2D collision)
    {
        switch(collision.tag)
        {
            case "Bullet":
                BulletController bullet = collision.GetComponent<BulletController>();
                OnHit(bullet.damage);
                //Destroy(collision.gameObject);
                PoolManager.Despawn(collision.gameObject);
                break;
        }
    }

    public void OnHit(int damage)
    {
        hp -= damage;
        if (hp <= 0)
        {
            //Destroy(gameObject);
            PoolManager.Despawn(gameObject);
            GameManager.Instance.AddScore(score);

            int randItem = UnityEngine.Random.Range(0, aryItem.Length);
            //Instantiate(aryItem[randItem], _myTF.position, Quaternion.identity);
            PoolManager.Spawn(aryItem[randItem].gameObject, _myTF.position, Quaternion.identity);
        }
        else
        {
            _renderer.sprite = _sprite[1];
            Invoke("ReturnSprite", 0.05f);
        }
    }

    void ReturnSprite()
    {
        _renderer.sprite = _sprite[0];
    }
}
```

#### ItemController
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public enum ItemType
{
    none,
    coin,
    boom,
    power
}

public class ItemController : MonoBehaviour
{
    public Transform myTF;
    public Rigidbody2D rd;
    public float speed;
    public ItemType type;

    private void OnEnable()
    {
        rd.velocity = -myTF.up * speed;
    }
}
```

#### UI_Life
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class UI_Life : MonoBehaviour
{
    public GameObject[] aryObjLife;

    public void OnEnable()
    {
        GameManager.Instance.onLifeChange += UpdateUI;
    }

    public void OnDisable()
    {
        GameManager.Instance.onLifeChange -= UpdateUI;
    }

    private void UpdateUI(int life)
    {
        for (int i = 0; i<3; i++)
        {
            aryObjLife[i].SetActive(i < life);
        }
    }
}
```

#### UI_Score
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class UI_Score : MonoBehaviour
{
    public Text textScore;

    public void OnEnable()
    {
        GameManager.Instance.onScoreChange += UpdateUI;
    }

    public void OnDisable()
    {
        GameManager.Instance.onScoreChange -= UpdateUI;
    }

    private void UpdateUI(int score)
    {
        textScore.text = score.ToString();
    }
}
```

#### PoolManager
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Animations;
using UnityEngine.WSA;

public class PoolManager
{
    private static Dictionary< int, List<GameObject> > dictObject = new Dictionary<int, List<GameObject> >();
    private static Dictionary<int, GameObject> dictFolder = new Dictionary<int, GameObject>();

    public static GameObject Spawn(GameObject obj, Vector3 pos, Quaternion rotation)
    {
        if (dictObject.ContainsKey( obj.GetInstanceID() ) == false) //딕셔너리에서 리스트를 찾을 수 없다면
        {
            //새로운 리스트와 오브젝트 생성 후 딕셔너리에 삽입
            List<GameObject> newListObject = new List<GameObject>();
            GameObject clone2 = Object.Instantiate(obj, pos, rotation);
            newListObject.Add(clone2);
            dictObject.Add(obj.GetInstanceID(), newListObject);

            //폴더 생성
            GameObject newFolder = new GameObject(obj.name);
            dictFolder.Add(obj.GetInstanceID(), newFolder);
            Object.DontDestroyOnLoad(newFolder);
            clone2.transform.parent = newFolder.transform;

            return clone2;
        }

        //딕셔너리에서 리스트를 찾았다면
        List<GameObject> listObject = dictObject[obj.GetInstanceID()];
        GameObject folder = dictFolder[obj.GetInstanceID()];
        for (int i = 0; i < listObject.Count; i++)
        {
            if (listObject[i].activeSelf == false) //리스트에 가용 오브젝트가 있다면
            {
                //오브젝트 재사용
                GameObject gameObject = listObject[i];
                gameObject.transform.position = pos;
                gameObject.transform.rotation = rotation;
                gameObject.SetActive(true);
                return gameObject;
            }
        }

        //리스트에 가용 오브젝트가 없다면 오브젝트 추가
        GameObject clone = Object.Instantiate(obj, pos, rotation);
        clone.transform.parent = folder.transform;
        listObject.Add(clone);
        return clone;
    }

    public static void Despawn(GameObject obj)
    {
        obj.SetActive(false);
    }
}
```

#### BackgroundManager
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BackgroundController : MonoBehaviour
{
    public float speed;
    public Transform myTF;
    private Vector3 newPos = new Vector3(0, 0, 0);

    // Update is called once per frame
    void Update()
    {
        newPos.y = -speed * Time.deltaTime;
        if (myTF.position.y <= -12)
        {
            newPos.y += 12;
        }
        myTF.Translate(newPos); //myTF.position += newPos 와 동일함
    }
}

```