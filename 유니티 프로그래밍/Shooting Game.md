#2D
![[ShootingGame.png]]
![[ShootingGame2.png]]
# Inspectors
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
# Animators
![[ShootingGame_Animator.png]]
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
                        Instantiate(_bullet1, _myTF.position, _myTF.rotation);
                        break;
                    case 1:
                        Instantiate(_bullet1, _myTF.position + new Vector3(-0.1f, 0, 0), _myTF.rotation);
                        Instantiate(_bullet1, _myTF.position + new Vector3(0.1f, 0, 0), _myTF.rotation);
                        break;
                    default:
                        Instantiate(_bullet1, _myTF.position + new Vector3(-0.25f, 0, 0), _myTF.rotation);
                        Instantiate(_bullet2, _myTF.position, _myTF.rotation);
                        Instantiate(_bullet1, _myTF.position + new Vector3(0.25f, 0, 0), _myTF.rotation);
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
        if (_invincible)
        {
            return;
        }

        switch(collision.tag)
        {
            case "EnemyBullet":
            case "Enemy":
                Destroy(collision.gameObject);
                gameObject.SetActive(false); //화면에서 사라짐
                Invoke("ShowPlayer", 2.0f);
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

public class GameManager : MonoBehaviour
{
    public static GameManager Instance;
    public Transform _myTF;
    public Transform _playerTF;
    public Transform[] arySpawn;
    public Transform[] aryEnemy;
    public float _spawnCooldown;
    private float _cooldown;

    private void Awake()
    {
        Instance = this;
    }

    private void Update()
    {
        if (_cooldown <= 0)
        {
            _cooldown += _spawnCooldown;
            int randSpawn = Random.Range(0, arySpawn.Length);
            int randEnemy = Random.Range(0, aryEnemy.Length);

            Vector2 direction = _playerTF.position - arySpawn[randSpawn].position;
            float angle = Mathf.Atan2(direction.y, direction.x) * Mathf.Rad2Deg;
            Quaternion rotation = Quaternion.identity;
            rotation.eulerAngles = new Vector3(0, 0, angle + 90);

            Instantiate(aryEnemy[randEnemy], arySpawn[randSpawn].position, rotation);
        }
        else
        {
            _cooldown -= Time.deltaTime;
        }
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
                Destroy(collision.gameObject);
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

public class PlayerBulletController : MonoBehaviour
{
    public float _speed;
    public Rigidbody2D _rd;
    public Transform _myTF;

    // Start is called before the first frame update
    void Start()
    {
        _rd.AddForce(_myTF.up * _speed, ForceMode2D.Impulse);
    }

    private void Update()
    {
        if (_myTF.position.y > 5.5f)
        {
            Destroy(gameObject);
        }
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

    void Start()
    {
        _rd.velocity = -_myTF.up * _speed;
    }

    private void Update()
    {
        if ( _bulletTF == null || !IsInScreen() )
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

            Instantiate(_bulletTF, _myTF.position, rotation);
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
                Destroy(collision.gameObject);
                break;
        }
    }

    void OnHit(int damage)
    {
        hp -= damage;
        if (hp <= 0)
        {
            Destroy(gameObject);
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