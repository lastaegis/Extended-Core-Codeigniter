<?php

/**
 * Created by PhpStorm.
 * User: ian-nano
 * Date: 01/10/16
 * Time: 23:10
 */
class L_Model extends CI_Model
{
    protected $tableName        = '';
    protected $primaryKey       = '';
    protected $orderBy          = '';
    protected $directionOrderBy = '';

    function __construct()
    {
        parent::__construct();
    }

    /**
     * Digunakan untuk mengambil data secara keseluruhan tanpa menggunakan parameter apapun
     *
     * @param null $selectList
     * @param null $idTable
     * @param null $limit
     * @param string $returnType
     * @return mixed
     */
    public function getData($selectList = null, $idTable = null, $limit = null, $returnType = 'text')
    {
        if($selectList != null)
        {
            $this->db->select(array_map(null,$selectList));
        }
        else
        {
            $this->db->select('*');
        }
        $this->db->from($this->tableName);
        if($idTable != null)
        {
            $this->db->where($this->primaryKey, $idTable);
        }
        if($this->orderBy != null && $this->directionOrderBy != null)
        {
            $this->db->order_by($this->orderBy, $this->directionOrderBy);
        }
        $this->db->limit($limit);
        $query = $this->db->get();
        if($returnType == 'json')
        {
            echo $this->_returnTypeParser($query, $returnType);
        }
        else
        {
            return $this->_returnTypeParser($query, $returnType);
        }
    }

    /**
     * Digunakan untuk mengambil data berdasarkan parameter yang diberikan
     *
     * @param null $selectList
     * @param null $whereCondition
     * @param null $limit
     * @param string $returnType
     * @return mixed
     */
    public function getDataBy($selectList = null, $whereCondition = null, $limit = null, $returnType = 'text')
    {
        if($selectList != null)
        {
            $this->db->select(array_map(null,$selectList));
        }
        else
        {
            $this->db->select('*');
        }
        $this->db->from($this->tableName);
        if($whereCondition != null)
        {
            foreach ($whereCondition as $key=>$value)
            {
                $this->db->where($key, $value);
            }
        }
        $this->db->limit($limit);
        $query = $this->db->get();
        if($returnType == 'json')
        {
            echo $this->_returnTypeParser($query, $returnType);
        }
        else
        {
            return $this->_returnTypeParser($query, $returnType);
        }
    }

    /**
     * Digunakan untuk menyimpan / mengupdate data didalam database
     *
     * @param $dataInbound
     * @param null $idTable
     * @return mixed
     */
    public function saveData($dataInbound, $idTable = null)
    {
        if($idTable == null)
        {
            $this->db->insert($this->tableName, $dataInbound);
            return $this->db->insert_id();
        }
        else
        {
            $this->db->where($this->primaryKey, $idTable);
            $this->db->update($this->tableName, $dataInbound);
        }
    }

    public function deleteData($idTable, $dataInbound = null)
    {
        if($idTable != null)
        {
            $this->db->where($this->primaryKey, $idTable);
            $this->db->delete($this->tableName);
        }
        else
        {
            foreach ($dataInbound as $key=>$value)
            {
                $this->db->where($key, $value);
            }
            $this->db->delete($this->tableName);
        }
    }

    public function datatables($draw, $start, $length, $selectList = array(), $whereCondition = array(), $join = array(), $orderBy, $direction)
    {
        //$recordsTotal   = $this->_countAllRecord();
        //$filteredRecord = $this->_countFilteredRecord($where);
        $data           = array();
        $this->db->select('kokab_nama, provinsi_nama');
        $this->db->from('master_kokab mk');
        $this->db->join('master_provinsi mp','mp.provinsi_id = mk.provinsi_id');
        if($whereCondition != null)
        {
            foreach ($whereCondition as $condition)
            {
                if($condition['value'] !== "")
                {
                    $this->db->like($condition['column'], $condition['value']);
                }
            }
        }
        if($orderBy != null)
        {
            $this->db->order_by($orderBy, $direction);
        }
        $this->db->limit($length, $start);
        $query = $this->db->get();

        foreach ($query->result() as $result)
        {
            $dataResult = array(
                'kokab_nama'    => $result->kokab_nama,
                'provinsi_nama' => $result->provinsi_nama,
                'control'       => '
                    <a href="#"><i class="fa fa-eye"></i></a> || <a href=""><i class="fa fa-edit"></i></a> || <a href="#"><i class="fa fa-trash"></i></a>
                '
            );

            array_push($data, $dataResult);
        }

        //$jsonReturn = array(
        //    "draw"              => (int)$draw,
        //    "recordsTotal"      => $recordsTotal,
        //    "recordsFiltered"   => $filteredRecord,
        //    "data"              => $data
        //);

        //return json_encode($jsonReturn);
    }

    /**
     * Digunakan untuk menghitung jumlah data dalam sebuah table
     */
    public function countAllData()
    {
        $this->db->select($this->primaryKey);
        $this->db->from($this->tableName);
        return $this->db->count_all_results();
    }

    private function _countFilteredData()
    {

    }

    private function _returnTypeParser($returnQuery, $returnType)
    {
        switch ($returnType){
            case "json":
                echo json_encode($returnQuery->result());
                break;
            case "text":
                return $returnQuery->result();
                break;
            case "array":
                return $returnQuery->result_array();
                break;
            default:
                break;
        }
    }
}
