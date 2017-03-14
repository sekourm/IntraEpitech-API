# IntraEpitech-API

<pre>

sudo chmod 777 * -R

</pre>

<code>
class CurlWrapper {

    private $curlopt = array(
        CURLOPT_RETURNTRANSFER => 1,
        CURLOPT_SSL_VERIFYHOST => false,
        CURLOPT_SSL_VERIFYPEER => false,
        CURLOPT_FOLLOWLOCATION => true,
        CURLOPT_HEADER => false,
    );
    private $cookie = '';
    private $lastP = '';
    private $lastI = '';

    public function __construct ($_cookie) {
        $this->cookie = $_cookie;
    }

    public function classic_connect ($cookie, $login, $password) {
        $c = curl_init('https://intra.epitech.eu');
        curl_setopt($c, CURLOPT_RETURNTRANSFER, 1);
        curl_setopt($c, CURLOPT_COOKIEFILE, $cookie);
        curl_setopt($c, CURLOPT_COOKIEJAR, $cookie);
        curl_setopt($c, CURLOPT_USERAGENT, 'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; .NET CLR 1.0.3705; .NET CLR 1.1.4322; Media Center PC 4.0)');
        curl_setopt($c, CURLOPT_FOLLOWLOCATION, true);
        curl_setopt($c, CURLOPT_HEADER, true);
        curl_setopt($c, CURLOPT_POST, true);
        curl_setopt($c, CURLOPT_POSTFIELDS, 'login=' . $login . '&password=' . urlencode($password) . '&remind=true&format=json');
        curl_setopt($c, CURLOPT_SSL_VERIFYHOST, false);
        curl_setopt($c, CURLOPT_SSL_VERIFYPEER, false);
        $page = curl_exec($c);
        $infos = curl_getinfo($c);

        $http_code = $infos['http_code'];

        curl_close($c);

        return $http_code;
    }

    public function get ($url, $cookie = false) {
        $c = curl_init($url);
        foreach ($this->curlopt as $key => $value) {
            curl_setopt($c, $key, $value);
        }
        if ($cookie) {
            curl_setopt($c, CURLOPT_COOKIEFILE, $this->cookie);
        }
        $p = curl_exec($c);
        $this->lastP = $p;
        $this->lastI = curl_getinfo($c);
        curl_close($c);
        return $p;
    }

    public function post ($url, $params) {
        $c = curl_init($url);
        $fields_string = '';
        foreach ($this->curlopt as $key => $value) {
            curl_setopt($c, $key, $value);
        }
        foreach($params as $key=>$value) { $fields_string .= $key.'='.urlencode($value).'&'; }
        rtrim($fields_string, '&');
        curl_setopt($c, CURLOPT_COOKIEFILE, $this->cookie);
        curl_setopt($c, CURLOPT_COOKIEJAR, $this->cookie);
        curl_setopt($c, CURLOPT_POST, count($params));
        curl_setopt($c, CURLOPT_POSTFIELDS, $fields_string);
        $p = curl_exec($c);
        $this->lastP = $p;
        $this->lastI = curl_getinfo($c);
        curl_close($c);
        return $p;
    }

    public function __toString() {
        return $this->lastP;
    }

    public function getHttpResponseCode () {
        return $this->lastI['http_code'];
    }

    public function getAllResponseHeaders () {
        return $this->lastI;
    }
}


$te = new CurlWrapper('cookie');
$te->classic_connect('cookie','xxxxx@epitech.eu','xxxxxx');

var_dump($te->get('https://intra.epitech.eu/user?format=json', true));

</code>
