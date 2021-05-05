# TugasMobile2
class PenjualanBarang {
  PenjualanBarang({
    this.no,
    this.nama_barang,
    this.jumlah,
    this.harga,
  });

  String no;
  String nama_barang;
  String jumlah;
  int harga;
}

import 'package:explore_data/useCase/sqflite/PenjualanBarang.dart';
import 'package:path/path.dart';
import 'package:sqflite/sqflite.dart';

class MySqflite {
  static final _databaseName = "MyDatabase.db";

  static final _databaseV1 = 1;
  static final tablepenjualan = 'penjualan';

  static final columnno = 'no';
  static final columnnama_barang= 'nama barang';
  static final columnjumlah = 'jumlah';
  static final columnharga = 'harag';

  // make this a singleton class
  MySqflite._privateConstructor();

  static final MySqflite instance = MySqflite._privateConstructor();

  static Database _database;

  Future<Database> get database async {
    if (_database != null) return _database;
    _database = await _initDatabase();
    return _database;
  }

  _initDatabase() async {
    var databasesPath = await getDatabasesPath();
    String path = join(databasesPath, _databaseName);

    return await openDatabase(path, version: _databaseV1,
        onCreate: (db, version) async {
      var batch = db.batch();
      _onCreateTableMahasiswa(batch);

      await batch.commit();
    });
  }

  void _onCreateTablePenjualan(Batch batch) async {
    batch.execute('''
          CREATE TABLE $tablePenjualan (
            $columnNim TEXT PRIMARY KEY,
            $columnName TEXT,
            $columnDepartment TEXT,
            $columnSKS INTEGER
          )
          ''');
  }

  ///TABLE penjualan
  Future<int> insertMahasiswa(PenjualanBarang barang) async {
    var row = {
      columnno: model.no,
      columnnama_barang: model.nama_barang,
      columnjumlah: model.jumlah,
      columnharga: model.harga
    };

    Database db = await instance.database;
    return await db.insert(tablePenjualan, row);
  }

  Future<List<PenjualanBarang>> getPenjualan() async {
    Database db = await instance.database;
    var allData = await db.rawQuery("SELECT * FROM $tablePenjualan");

    List<PenjualanBarang> result = [];
    for (var data in allData) {
      result.add(PenjualanBarang(
          no: data[columnno],
          nama_barang: data[columnnama_barang],
          jumlah: data[columnjumlah],
          harga: int.parse(data[columnharga].toString())));
    }

    return result;
  }

  Future<PenjualanBarang> getPenjualanByno(String nim) async {
    Database db = await instance.database;
    var allData = await db.rawQuery(
        "SELECT * FROM $tablePenjualan WHERE $columnni = $no LIMIT 1");

    if (allData.isNotEmpty) {
      return PenjualanBarang(
          no: allData[0][columnno],
          nama_barang: allData[0][columnnama_barang],
          Jumlah: allData[0][columnjumlah],
          Harga: int.parse(allData[0][columnharga]));
    } else {
      return null;
    }
  }

  Future<int> updateOenjualanBarang(PenjualanBarang Barang) async {
    Database db = await instance.database;
    return await db.rawUpdate(
        'UPDATE $tablePenjualan SET $columnjumlah = ${model.jumlah} '
        'Where $columnno = ${model.no}');
  }

  Future<int> deletePenjualan(String jumlah) async {
    Database db = await instance.database;
    return await db
        .rawDelete('DELETE FROM $tablePenjualan Where $columnno = $);
  }

  clearAllData() async {
    Database db = await instance.database;
    await db.rawQuery("DELETE FROM $tablePenjualan");
  }
}

import 'package:explore_data/useCase/sqflite/PenjualanBarang.dart';
import 'package:explore_data/useCase/sqflite/MySqlflite.dart';
import 'package:flutter/material.dart';

class SqfliteActivity extends StatefulWidget {
  @override
  State<StatefulWidget> createState() => SqfliteActivityState();
}

class SqfliteActivityState extends State<SqfliteActivity> {
  final keyFormPe
