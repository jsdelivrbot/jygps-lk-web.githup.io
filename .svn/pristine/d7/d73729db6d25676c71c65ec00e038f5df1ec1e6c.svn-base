import axios from 'axios';
import qs from 'qs';

let base = 'http://114.55.138.209:8091/jygpsTmp';
let instance = axios.create({
   headers: {'content-type': 'application/x-www-form-urlencoded'},
   withCredentials: true

   })
   
//    登录页logo
export const requestLogin = params => { return instance.post(`${base}/userLogin`, qs.stringify(params)).then(res => res.data); };
// 修改密码
export const changePassword = params => { return instance.post(`${base}/sysuserinfo/changePsw`, qs.stringify( params )).then(res => res.data);  };
// 获取所有节点名
export const allNode = params => { return instance.post(`${base}/getAllNodes`, qs.stringify({ params: params })); };
export const unitTree = params => { return instance.get(`${base}/sysunitinfo/getCombUnit`,qs.stringify({ params: params })); };
export const nodeinfo = params => { return instance.post(`${base}/sysnodeinfo/list`, qs.stringify( params )).then(res => res.data); };
export const timers = params => { return instance.post(`${base}/vehicletmpdetailinfo/list`, qs.stringify( params )).then(res => res.data); };
export const equipment = params => { return instance.post(`${base}/sysnodeinfo/list`, qs.stringify( params )); };
export const history = params => { return instance.post(`${base}/vehicletmpdetailinfo/list`, qs.stringify( params )).then(res => res.data); };
export const saveWarnBatch = params => { return instance.post(`${base}/sysnodeinfo/saveWarnBatch`, qs.stringify( params )).then(res => res.data); };
export const getWarnData = params => { return instance.post(`${base}/sysnodeinfo/getWarnDataByNodeId`, qs.stringify( params )).then(res => res.data); };
export const getNode = params => { return instance.post(`${base}/sysnodeinfo/getNode`, qs.stringify( params )).then(res => res.data); };
export const excels = params => { return instance.post(`${base}/vehicletmpdetailinfo/exportExcel`, qs.stringify( params )).then(res => res.data); };
export const marks = params => { return instance.post(`${base}/sysnodeinfo/geWareHouseAddress`, qs.stringify( params )).then(res => res.data); };
export const getChart = params => { return instance.post(`${base}/vehicletmpdetailinfo/getChart`, qs.stringify( params )).then(res => res.data); };
export const nodeExcel = params => { return instance.post(`${base}/sysnodeinfo/exportExcel`, qs.stringify( params )).then(res => res.data); };
export const Alarmrecord = params => { return instance.post(`${base}/syswarningsendinfo/list`, qs.stringify( params )).then(res => res.data); };
export const vehicletmpdetailinfo = params => { return instance.post(`${base}/vehicletmpdetailinfo/getTmpHuiZong`, qs.stringify( params )).then(res => res.data); };
export const Userlist = params => { return instance.post(`${base}/sysuserinfo/list`, qs.stringify( params )).then(res => res.data); };
export const sysuserinfoSave = params => { return instance.post(`${base}/sysuserinfo/save`, qs.stringify( params )).then(res => res.data); };
export const sysuserinfogetUser = params => { return instance.post(`${base}/sysuserinfo/getUser`, qs.stringify( params )).then(res => res.data); };
export const sysuserinfodeletUser = params => { return instance.post(`${base}/sysuserinfo/delete`, qs.stringify( params )).then(res => res.data); };
